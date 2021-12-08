---
Title: NestJs
---
# Creating a nestjs project from scratch

  

- `nest new $PROJECT_NAME`

  

## (optional) remove eslint tsconfig errors

Add to `.eslintrc.js`:

- `tsconfigRootDir: __dirname,` to parserOptions

- OR `"project": "$PROJECT_NAME/tsconfig.json"`

  

## Create skel

1. Create modules

- `nest g module users`

- `nest g module reports`

  

2. Create controllers

- `nest g controller users`

- `nest g controller reports`

  

3. Create services

- `nest g service users`

- `nest g service reports`

  

## Create repositories

- `npm install @nestjs/typeorm typeorm sqlite3 --save`

Nestjs and typeorm handle repository creation behind the scenes, but we DO have to create entities from which the repositories will be created:

  

### Create entities

1. create a file `user.entity.ts`

2. `export class User {}`

3. `import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm'`

4. Add decorators: `@Entity()`, `@PrimaryGeneratedColumn` and `@Column`

5. Add import `TypeOrmModule.forFeature([User])` to `users.module.ts` imports array

6. Add import to `app.module.ts` imports array

```js

 TypeOrmModule.forRoot({

 type: 'sqlite',

 database: './db.sqlite',

 entities: [User],

 synchronize: true,

 })

```

  

## Create GlobalValidationPipe

- add `app.useGlobalPipes(new ValidationPipe({ whitelist: true }))` to `main.ts`

- Whitelist: true means @Body() req. will be strictly parsed to the DTOs ie. given the request:

- `{ email: 'dani@email.com', password: 'mypass', admin: 'true' }`

- Only the fields defined in our DTO will be saved (`admin: true` is stripped)

- `{ email: 'dani@email.com', password: 'mypass' }`

  

## Install dependencies for class-validator and class-transformer

- `npm i class-validator class-transformer --save`

  

## Create DTOs

- `create-user.tdo.ts` ->

```js

import { IsEmail, IsString } from 'class-validator';

  

export class CreateUserDto {

 @IsEmail()

 email: string;

  

 @IsString()

 password: string;

}

```

  

## Implement create-user.dto inside our controller

- `createUser(@Body() body: CreateUserDto) {}`

  

## Implement the repository inside our service

```js

import { Repository } from 'typeorm';

import { InjectRepository } from '@nestjs/typeorm';

import { User } from './user.entity';

  

@Injectable()

export class UsersService {

 constructor(@InjectRepository(User) private repo: Repository<User>) {}

}

```

  

## this.repo.create({ })

Returns a User class instance -> needed for any entity hooks like @AfterInsert, @AfterUpdate etc..

  

## this.repo.save(user)

Saves instance to the persistance layer (DB) -> this executes the hooks

  

## update() and remove() methods

These methods need their own DTOs or make use of TS Partial<User> types:

- `update(id: number, attrs: Partial<User>) {}`

  

## Object.assign to copy the update-user.dto / attrs

```js
 const user = await this.findOne(id);

 if (!user) {

 throw new Error('User not found');

 }

 Object.assign(user, attrs);

return this.repo.save(user);
```

# Implementing controller routes

- Important -> Parse `id` from `string` to `int` 3-ways:

 1. `.findOne(parseInt(id))`

 2. `.findOne(+id)`

 3. ParseIntPipe `findOne(@Param('id', ParseIntPipe) id: number) {`

```js

 @Get('/:id')

 findUser(@Param('id') id: string) {

 return this.usersService.findOne(+id);

 }

```

  

## Create an update-user.dto.ts file for the updateUser() method -> use `@IsOptional` decorator

```js

import { IsEmail, IsString, IsOptional } from 'class-validator';

  

export class UpdateUserDto {

 @IsEmail()

 @IsOptional()

 email: string;

  

 @IsString()

 @IsOptional()

 password: string;

}

```

  

# Handle HTTP specific errors by importing from @nestjs/common

- `{ NotFoundException }` etc.

- `throw new NotFoundException('User not found');`

  

# Interceptors to exclude response properties (ie. password fields)

- `import { Exclude } from 'class-transformer'` in `user.entity.ts`

- add `@Exclude()` to the password property

  

Then:

- `Import { UseInterceptors, ClassSerializerInterceptor } from '@nestjs/common'` in `users.controller.ts`

- Add `@UseInterceptors(ClassSerializerInterceptor)` to the @Get req.

  

## Implement custom interceptor DTOs for route-based serialization

Sometimes the default interceptor isn't the best choice.. we sometimes want to hand out **MORE** information about an entity to a particular route ('/admin' vs '/users') or token.

- A custom interceptor is a kind of *Middleware* from Express.

- Interceptors can be applied to a single handler, all the handlers in a controller, or globally

- We define them in a `class CustomInterceptor` in which we MUST define a `intercept()` method

- A `intercept()` method has 3 parts:

`intercept( context: ExecutionContext, next: CallHandler )`

 1. the 'intercept' method which is called automatically

 2. context (Information on the incoming request)

 3. next (_Kind of_ a reference to the request handler in our controller)

- `serialize.interceptor.ts`

```js

import {

 UseInterceptors,

 NestInterceptor,

 ExecutionContext,

 CallHandler,

} from '@nestjs/common';

import { Observable } from 'rxjs';

import { map } from 'rxjs/operators';

import { plainToClass } from 'class-transformer';

  

export class SerializeInterceptor implements NestInterceptor {

 intercept(context: ExecutionContext, next: CallHandler): Observable<any> {

 // Run something before the request is handled

 // by the request handler (controller)

 console.log("I'm running before the handler", context);

  

 return next.handle().pipe(

 map((data: any) => {

 // Run something before the response is sent out

 console.log("I'm running before response is sent out", data);

 // plainToClass(context.getClass(), data);

 }),

 );

 }

}

```

- Add `@UseInterceptors(SerializeInterceptor)` to `users.controller.ts`

- Create `user.dto.ts` and `@Expose()` the id, email properties

- Update `serialize.interceptor.ts` to use the UserDto class as a `plainToClass` class-transformer obj. `excludeExtraneousValues` means drop password and others not *EXPLICITLY* decorated with `@Expose()`.

```js

 return next.handle().pipe(

 map((data: any) =>

 plainToClass(UserDto, data, { excludeExtraneousValues: true }),

 ));

```

- In order to re-use the interceptor with many different DTOs we have to implment a constructor and pass the desired DTO to apply the transformation based on.

- in `serialize.interceptor.ts`

- `constructor(private dto: any) {}`

- modify `plainToClass` as `plainToInstance( this.dto, data, { excludeExtraneousValues: true })`

- back in `users.controller.ts` -> `@UseInterceptors(new SerializeInterceptor(UserDto))` make use of the new class constructor functionality by passing the user.dto

  

# Wrapping the interceptor in a (custom) decorator

A decorator @ is basically a function

1. we take `@UseInterceptors(new SerializeInterceptor(UserDto))` from `users.controller.ts`

2. add it to the `serialize.interceptor.ts` basically as-is (just passing this.dto from the instance property) as a **function**.

```js

export function Serialize(dto: any) {

 return UseInterceptors(new SerializeInterceptor(dto));

}

```

3. add `@Serialize(UserDto)` back to the route handler inside the `users.controller.ts`

4. (Optional) Move the decorator to the TOP of the controller

  

# Implementing Auth

- Can be done inside the users.service but for better separation of concern and reusability it is best to create a new `auth.service.ts`

- create AuthService class, add `@Injectable()` decorator and pass `UsersService` to the constructor to setup DI.

- Add AuthService to the providers: array in `users.module.ts`

- Import `randomBytes` and `scrypt` (`promisify`)

```js

 async signup(email: string, password: string) {

 // See if email is in use

 const users = await this.usersService.find(email);

 if (users.length) {

 throw new BadRequestException('Email already in use');

 }

 // Hash the users password

 // Generate a salt

 const salt = randomBytes(8).toString('hex');

 // Hash the salt and the password together

 const hash = (await scrypt(password, salt, 64)) as Buffer;

 // Join the hashed result and the salt together

 const hashedPassword = salt + '.' + hash.toString('hex');

 // Create a new user and save it

 const user = await this.usersService.create(email, hashedPassword);

 // return the new user

 return user;

 }

```

  

```js

 async signin(email: string, password: string) {

 const [user] = await this.usersService.find(email);

 if (!user) {

 throw new NotFoundException('User not found');

 }

 // Split the hashed password into its parts

 const [salt, storedHash] = user.password.split('.');

 // Hash the password that the user is trying to log in with

 const hash = (await scrypt(password, salt, 64)) as Buffer;

 // Compare the hashed password to the hashed password in the database

 if (storedHash !== hash.toString('hex')) {

 throw new BadRequestException('Invalid password');

 }

 // Return the user

 return user;

 }

```

  

# Setting up Sessions

- `npm i --save cookie-session @types/cookie-session`

  

# Guards // Decorators & Interceptors

  

## Custom decorator for getting the current user

```js

import { createParamDecorator, ExecutionContext } from '@nestjs/common';

  

export const CurrentUser = createParamDecorator(

 (data: never, context: ExecutionContext) => {

 const request = context.switchToHttp().getRequest();

 console.log(request.session.userId);

 return "hi there";

 },

);

```

- ExecutionContext is synonymous with (HTTP) Request, but because it applies to other micro-services transport layers like websockets, gRPC, GraphQL

- data type `never` -> this value is never going to be used, accessed in any shape or form

- then import the decorator in `controller` and

```js

 @Get('/whoami')

 whoAmI(@CurrentUser() user: string) {

 return user;

 }

```

- we also need an interceptor with our created decorator because **Param decorators exist *outside* the DI system, so our decorator can't get an instance of UsersService directly**

- The interceptor `CurrentUserInterceptor` will get the current User based on the session Object userId from the DI injected UsersService>UsersRepo and expose it to the decorator

  

# e2e testing

For end-to-end testing we need to have our ENTIRE app bootstrapped inside the `app.module.ts` file which is the one jest defaults to creating an app *mock* from.

Two ways of doing that:

1. Extract the Global ValidationPipe and cookieSession middleware to a JS file and import and bootstrap it from within our `app.module.ts` file -> `setupApp(app)`

```js

export const setupApp = (app: any) => {

 app.use(

 cookieSession({

 keys: ['whatever'],

 }),

 );

 app.useGlobalPipes({

 new ValidationPipe({

 whitelist: true

 }),

 });

};

```

2. Use GLOBALLY scoped pipe for the Validation pipe in `app.module.ts`

- import { ValidationPipe } from '@nestjs/common'

- import { APP_PIPE } from '@nestjs/core'

```js

providers: [

 AppService,

 {

 provide: APP_PIPE,

 useValue: new ValidationPipe({

 whitelist: true

 })

 },

]

```

To then export the remaining cookie-session middleware we have to add a function to the class `AppModule` export at the end:

```js

export class AppModule {

 configure(consumer: MiddlewareConsumer) {

 consumer.apply(

 cookieSession({

 keys: ['mySecret'],

 })

 )

 .forRoutes('*');

 }

}

```

this `cofigure()` function is going to be automatically executed whenever our application starts listening to traffic, so what's inside the function will run on every single request ( `.forRoutes('*')` )

  

# Setting up env variables the Nest way (`@nestjs/config`)

- `npm i --save @nestjs/config`

- create separate .env files for dev/test with different `DB_NAME=xxx` values

- import nest config into `app.module.ts`

- `import { ConfigModule, ConfigService } from '@nestjs/config';`

- ConfigModule -> decide which .env to use

- add to imports: []

```

ConfigModule.forRoot({

 isGlobal: true,

 envFilePath: `.env.${process.env.NODE_ENV}`

}),

```

- ConfigService -> expose that config to the rest of our app

```js

 TypeOrmModule.forRootAsync({

 inject: [ConfigService],

 useFactory: (config: ConfigService) => {

 return {

 type: 'sqlite',

 database: config.get<string>('DB_NAME'),

 entities: [User, Report],

 synchronize: true,

 };

 },

 }),

```

- setup TypeOrmConfig based on .env

- MAKE SURE THAT **NODE_ENV** is set before each `npm run xxx` command

- `npm i cross-env`

-> update package.json run commands with `"start": "cross-env NODE_ENV=development nest start"`

  

# TESTING: Enable and use *global* before all statement (ie. delete test DB)

- Add to `jest-e2e.json` ->

- `"setupFilesAfterEnv": ["<rootDir>/setup.ts"]` <- execute BEFORE all tests

```js

// setup.ts

import { rm } from 'fs/promises';

import { join } from 'path';

  

global.beforeEach(async () => {

 try {

 await rm(join(__dirname, '..', 'test.sqlite'));

 } catch (error) {}  // do nothing with the error

});

```

- add an `afterEach()` statement to close the connection to the DB after each test

- in `setup.ts`

- `import { getConnection } from 'typeorm`

```js

global.afterEach(async () => {

 const conn = getConnection();

 await conn.close();

})

```

  

# Setting up associations `OneToOne()`, `OneToMany() / ManyToOne()`, `ManyToOne` in Nest and TypeOrm

  

1. Figure out what kind of assocation we are modeling

2. Add the appropriate decorators to our related entities

```js

// user.entity.ts

 @OneToMany(() => Report, (report) => report.user)

 reports: Report[];

  

// report.entity.ts

 @ManyToOne(() => User, (user) => user.reports)

 user: User;

```

3. Associate the records when one is created

```js

// reports.controller.ts

 @Post()

 @UseGuards(AuthGuard)

 createReport(@Body() body: CreateReportDto, @CurrentUser() user: User) {

 return this.reportsService.create(body, user);

 }

  

// reports.service.ts

 create(reportDto: CreateReportDto, user: User) {

 const report = this.repo.create(reportDto);

 report.user = user;

 return this.repo.save(report);

 }

```

4. Apply a Serializer to limit info shared (expose just userId instead of the whole user obj)

```js

// new file: report.dto.ts

  

import { Expose, Transform } from 'class-transformer';

  

export class ReportDto {

 @Expose()

 id: number;

  

 @Expose()

 price: number;

  

 @Expose()

 year: number;

  

 @Expose()

 lng: number;

  

 @Expose()

 lat: number;

  

 @Expose()

 make: string;

  

 @Expose()

 model: string;

  

 @Expose()

 mileage: number;

  

 @Transform(({ obj }) => obj.user.id)

 @Expose()

 userId: number;

}

```

- then add the @Serialize decorator to the reports route(s) controller passing in the DTO we've just created

```js

 @Post()

 @UseGuards(AuthGuard)

 @Serialize(ReportDto)

 createReport(@Body() body: CreateReportDto, @CurrentUser() user: User) {

 return this.reportsService.create(body, user);

 } 

```

  

# Adding in a basic permission system

- Add a `report.entity.ts` field `approved` with a default being `false`

```js

 @Column({ default: false })

 approved: boolean;

```

- Add a new PATCH route in the reports controller which will modify a report ? false : true

```js

 @Patch(':id')

 approveReport(@Param('id') id: string, @Body() body: ApproveReportDto) {

 return this.reportsService.changeApproval(id, body.approved);

 }

```

- Add and import an `ApprovedReportDto`

```js

import { IsBoolean } from 'class-validator';

  

export class ApproveReportDto {

 @IsBoolean()

 approved: boolean;

}

```

- implement the `.changeApproval()` method in `reports.service.ts`

```js

 async changeApproval(id: string, approved: boolean) {

 const report = await this.repo.findOne(id);

  

 if (!report) {

 throw new NotFoundException('Report not found');

 }

  

 report.approved = approved;

 return this.repo.save(report);

 }

```

- finally, make sure the report response DTO is updated with the new `approved` field

```js

// report.dto.ts

 @Expose()

 approved: boolean;

```

# Using TypeORM-CLI for db migrations

- First TypeORM needs to be able to read our .ts entity files

- `npm i -g ts-node` -> install ts-node (nestjs already does this)

- `"typeorm": "node --require ts-node/register ./node_modules/typeorm/cli.js"`

- `npm run typeorm migration:generate -- -n initial-schema --outputJs` -> generate schema from entities

- `npm run typeorm migration:run` -> run migration / create schema in DB

  

# Heroku deploy

```js

681  npm i pg

 heroku auth:whoami

 686  git add .

 687  git commit -m "pre-production commit"

 688  heroku create

 689  heroku addons:create heroku-postgresql:hobby-dev

 690  heroku config:set COOKIE_KEY=MySECRETkey123y75ujsbnfkjba

 691  heroku config:set NODE_ENV=production

 692  git push heroku master

```

- TODO:

- change `main.ts` PORT to read process.env

 - `await app.listen(process.env.PORT || 3000);`

- create heroku `Procfile`

 - `web: npm run start:prod`

- update `tsconfig.build.json` file to exclude .js files and migrations folder

 - `"exclude": ["ormconfig.js", "migrations", "node_modules", "test", "dist", "**/*spec.ts"]`

- (opt) comment out "incremental": true, in `tsconfig.json`
	
# ---

Tags: #typescript 


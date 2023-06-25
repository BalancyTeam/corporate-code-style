# NestJS Code Style Guide
This document outlines the recommended code style guidelines for writing **NestJS** applications, with a focus on using **TypeORM** and **PostgreSQL** for database interactions. Following a consistent code style helps improve readability, maintainability, and collaboration within your team.

## Table of Contents
- [General Principles](#general-principles)<br/>
- [Naming Conventions](#naming-conventions)<br/>
- [File Structure](#file-structure)<br/>
- [Indentation and Spacing](#indentation-and-spacing)<br/>
- [Imports](#imports)<br/>
- [Controllers](#controllers)<br/>
- [Services](#services)<br/>
- [Modules](#modules)<br/>
- [Dependency Injection](#dependency-injection)<br/>
- [Error Handling](#error-handling)<br/>
- [Testing](#testing)<br/>
- [Database](#database)<br/>
## General Principles
Consistency: Maintain consistent coding style throughout the project.
Readability: Write code that is easy to read and understand.
Modularity: Encourage modular code structure and reusability.
**SOLID** Principles: Aim to follow the **SOLID** principles (Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) to write maintainable code.
## Naming Conventions
Files and Directories: Use kebab-case for file and directory names (e.g., **user.service.ts**, **user.controller.ts**).
Classes: Use PascalCase for class names (e.g., **UserService**, **UserController**).
Variables and Functions: Use camelCase for variable and function names (e.g., **getUserById**, **userData**).
Interfaces: Use PascalCase with an I prefix for interface names (e.g., **IUser**, **IToken**).
Enums: Use PascalCase for enum names and **UPPER_SNAKE_CASE** for enum values (e.g., **UserRole**, **HTTP_STATUS**).
Constants: Use **UPPER_SNAKE_CASE** for constant names (e.g., **MAX_ATTEMPTS**, **DEFAULT_PORT**).
## File Structure
Organize your project files and directories based on their functionality and modules. A typical file structure for a **NestJS** application may look like:

```
/src
  /user
    user.module.ts
    user.service.ts
    user.controller.ts
    /dto
      create-user.dto.ts
      update-user.dto.ts
    /entities
      user.entity.ts
  /common
    /guards
      auth.guard.ts
    /interceptors
      logging.interceptor.ts
    /utils
      validation.util.ts
  app.module.ts
  main.ts
```
## Indentation and Spacing
Use 2 spaces for indentation. Do not use tabs.
Maintain a consistent vertical spacing between blocks of code to improve readability.
Use single spaces for separating operators and function arguments.
Leave a single empty line between class methods and logical sections of code.
## Imports
Import modules and external dependencies first, followed by internal imports.
Separate import statements into three blocks:
Standard Library Imports
Third-Party Imports
Internal Imports (relative paths)

```typescript
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

import { UserService } from './user.service';
import { Logger } from '../common/config/logger';
```
## Controllers
Use **kebab-case** for route names and **camelCase** for controller class names.
Use the appropriate HTTP method decorators (`@Get()`, `@Post()`, `@Put()`, `@Delete()`, etc.) for defining routes.
Keep the controller methods concise and focused on a specific task.
Use **DTO** (Data Transfer Objects) for validating and sanitizing incoming data.

```typescript
import { Controller, Get, Post, Body } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  getUsers() {
    return this.userService.getUsers();
  }

  @Post()
  createUser(@Body() createUserDto: CreateUserDto) {
    return this.userService.createUser(createUserDto);
  }
}
```
## Services
Use **PascalCase** for service class names.
Keep the service methods focused on a single responsibility.
Avoid complex logic in services and move it to dedicated helper classes or utilities if needed.
Use dependency injection to provide required dependencies.
```typescript
import { Injectable } from '@nestjs/common';
import { UserRepository } from '../repositories/user.repository';
import { CreateUserDto } from './dto/create-user.dto';
import { User } from './entities/user.entity';

@Injectable()
export class UserService {
  constructor(private readonly userRepository: UserRepository) {}

  async getUsers(): Promise<User[]> {
    return this.userRepository.find();
  }

  async createUser(createUserDto: CreateUserDto): Promise<User> {
    const user = this.userRepository.create(createUserDto);
    return this.userRepository.save(user);
  }
}
```
## Modules
Use **kebab-case** for module file names and **PascalCase for** module class names.
Maintain a separate module for each logical section of your application.
Use the `@Module()` decorator to define and configure modules.
Organize module imports by separating them into three blocks:
Internal Modules (your project modules)
External Modules (third-party modules)
Shared Modules (common modules used across your project)
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { UserController } from './user.controller';
import { UserService } from './user.service';
import { UserRepository } from '../repositories/user.repository';

@Module({
  imports: [
    ConfigModule,
    // Other internal modules
  ],
  controllers: [UserController],
  providers: [UserService, UserRepository],
})
export class UserModule {}
Dependency Injection
Use constructor-based dependency injection for injecting dependencies into classes.
Use the @Injectable() decorator for classes that rely on dependency injection.
Inject dependencies using constructor parameters.
typescript
Copy code
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class UserService {
  constructor(private readonly configService: ConfigService) {}

  // Service methods...
}
```
## Error Handling
Use **try-catch** blocks to handle and process errors.
Throw custom exceptions when necessary to provide meaningful error messages.
Use **NestJS** built-in exception filters to handle and format errors sent to the client.
```typescript
import { NotFoundException } from '@nestjs/common';

async getUserById(id: number): Promise<User> {
  try {
    const user = await this.userRepository.findOne(id);
    if (!user) {
      throw new NotFoundException('User not found');
    }
    return user;
  } catch (error) {
    throw new InternalServerErrorException('Failed to retrieve user');
  }
}
```
## Testing
Write unit tests for your modules, services, and controllers.
Use a testing framework such as Jest to write and execute tests.
Place your test files alongside the source code files they are testing.
Use test suites (describe) and test cases (it or test) to structure your tests.
```typescript
describe('UserService', () => {
  let userService: UserService;
  let userRepository: UserRepository;

  beforeEach(async () => {
    const moduleRef = await Test.createTestingModule({
      providers: [UserService, UserRepository],
    }).compile();

    userService = moduleRef.get<UserService>(UserService);
    userRepository = moduleRef.get<UserRepository>(UserRepository);
  });

  describe('getUsers', () => {
    it('should return an array of users', async () => {
      const users = [/* User objects */];
      jest.spyOn(userRepository, 'find').mockResolvedValue(users);

      const result = await userService.getUsers();
      expect(result).toEqual(users);
    });
  });

  // Other test cases...
});
```
## Database
When using **TypeORM** with **PostgreSQL** for database interactions, consider the following guidelines:

Define your entity classes to map database tables, using the appropriate decorators provided by TypeORM (`@Entity()`, `@Column()`, etc.).
Use repositories (`@Repository()`) to encapsulate database operations and provide a clean interface for interacting with entities.
Configure the database connection and TypeORM settings in the **ormconfig.js** or **ormconfig.json** file.
Leverage migrations to manage database schema changes over time. Use the typeorm migration:generate, typeorm migration:run, and typeorm migration:revert commands to create, apply, and revert migrations.
```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  firstName: string;

  @Column()
  lastName: string;
}
typescript
Copy code
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './entities/user.entity';

@Injectable()
export class UserRepository {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  find(): Promise<User[]> {
    return this.userRepository.find();
  }

  findById(id: number): Promise<User | undefined> {
    return this.userRepository.findOne(id);
  }

  create(user: User): Promise<User> {
    return this.userRepository.save(user);
  }
}
```
These guidelines provide a starting point for maintaining a consistent and readable code style in your **NestJS** projects, especially when using **TypeORM** with **PostgreSQL** for database interactions. Remember to adapt and extend these guidelines based on the specific needs and preferences of your team.
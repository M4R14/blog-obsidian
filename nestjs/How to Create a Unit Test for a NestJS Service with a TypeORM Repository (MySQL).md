NestJS is a great framework for building scalable and modular server-side applications. One of its key features is the ability to easily inject dependencies, including TypeORM repositories, into your service classes. In this tutorial, we'll show you how to create a unit test for a service that uses a TypeORM repository in a MySQL database.

First, let's take a look at how to inject the task repository into our service class:

```ts
// task.servic.ts

@Injectable()
export class TaskService {
  constructor(
    @InjectRepository(Task)
    private readonly taskRepository: Repository<Task>,
  ) {}
  ...
}
```
We can then use the TypeOrmModule to configure the database connection in our AppModule. However, for testing purposes, it's a good idea to separate this configuration into a separate file that can be imported into our testing module. This keeps our code clean and makes it easier to reuse for testing other parts of our application.

Here's an example of how to create a database configuration file for testing:

```ts
// src/test-utils/TypeORMMySqlTestingModule.ts

import { TypeOrmModule } from '@nestjs/typeorm';

export const TypeORMMySqlTestingModule = (entities: any[]) =>
  TypeOrmModule.forRoot({
    type: 'mysql',
    host: process.env.MYSQL_HOST || 'localhost',
    port: 3306,
    username: process.env.MYSQL_USERNAME || 'nest',
    password: process.env.MYSQL_PASSWORD || 'nest',
    database: process.env.MYSQL_DATABASE || 'test',
    entities: [...entities],
    synchronize: true,
  });
```
Then, in our testing module, we can import this configuration file and use it to set up our database connection:

```ts
// task.service.spac.ts
...
beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [
        TypeORMMySqlTestingModule([Task]),
        TypeOrmModule.forFeature([Task]),
      ],
      providers: [TaskService],
    }).compile();

    service = module.get<TaskService>(TaskService);
  });
...
```
Finally, we can run our tests by using the following command:
```sh
$ yarn test src/task/task.service.spec.ts
```

And that's it! You now have a fully functional unit test for a NestJS service with a TypeORM repository in a MySQL database.

For more information, check out the following resources:

- https://stackoverflow.com/a/55717767
- https://dev.to/webeleon/unit-testing-nestjs-with-typeorm-in-memory-l6m
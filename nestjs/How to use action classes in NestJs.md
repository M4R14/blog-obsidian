First, we'll create an `actions` folder in our project's root directory. Inside this folder, we'll create a file called `create-post.action.ts` with the following code:

```ts
import { Injectable } from '@nestjs/common';
import { Post } from './post.entity';
import { PostRepository } from './post.repository';

@Injectable()
export class CreatePostAction {
  constructor(private postRepository: PostRepository) {}

  async execute(data: any): Promise<Post> {
    const post = new Post();
    post.title = data.title;
    post.body = data.body;
    return await this.postRepository.save(post);
  }
}
```
In this code, we're creating a new `CreatePostAction` class that has a constructor that injects a `PostRepository` instance. We're also defining an execute method that takes in some data and returns a `Promise<Post>`.

Inside the execute method, we're creating a new Post instance, setting its title and body properties based on the data provided, and then saving it to the `PostRepository`.

Now that we've created the `CreatePostAction`, we'll use it in a controller. We'll assume that we have a `PostsController` that handles HTTP requests related to posts.

```ts
import { Controller, Post, Body } from '@nestjs/common';
import { CreatePostAction } from './actions/create-post.action';

@Controller('posts')
export class PostsController {
  constructor(private createPostAction: CreatePostAction) {}

  @Post()
  async create(@Body() data: any) {
    return await this.createPostAction.execute(data);
  }
}
```
In this code, we're injecting the `CreatePostAction` instance into the `PostsController` constructor. We're also defining a create endpoint that uses the `@Post` decorator and takes in some data from the HTTP request body.


Inside the `create` method, we're calling the `execute` method of the `CreatePostAction` instance, passing in the data from the HTTP request body. This will create a new Post instance and save it to the database.

Overall, using action classes can help make your code more modular and maintainable by separating business logic from controllers. By encapsulating specific actions in separate classes, you can reuse them across different controllers or applications, and also make your code more testable.

---
{% embed https://dev.to/m4r14/using-action-classes-in-nestjs-for-better-code-separation-and-reusability-1e8i %}

In NestJS, pipes are essential for data validation and transformation. They also help in handling errors gracefully. Let's focus on using pipes to tackle the issue of entity not found, with a simple example.

Imagine you're building an API with an endpoint to retrieve an item by its ID. If the item doesn't exist, you want to return an appropriate error response. Here's how you can do it using a custom pipe called **EntityExistsPipe**.

First, create the **EntityExistsPipe**:
```ts
import { PipeTransform, Injectable, NotFoundException } from '@nestjs/common';
import { ItemsService } from './items.service';

@Injectable()
export class EntityExistsPipe implements PipeTransform {
  constructor(private readonly itemsService: ItemsService) {}

  async transform(id: number) {
    const item = await this.itemsService.getItemById(id);
    if (!item) {
      throw new NotFoundException('Item not found');
    }
    return id;
  }
}
```

Next, apply the pipe to your route handler:

```ts
import { Controller, Get, Param, NotFoundException } from '@nestjs/common';
import { ItemsService } from './items.service';
import { EntityExistsPipe } from './entity-exists.pipe';

@Controller('items')
export class ItemsController {
  constructor(private readonly itemsService: ItemsService) {}

  @Get(':id')
  async getItem(@Param('id', EntityExistsPipe) id: number) {
    return this.itemsService.getItemById(id);
  }
}
```

In this setup, the **@Param** decorator binds the **id** parameter to the **getItem** method in the **ItemsController**. The **EntityExistsPipe** is used as the parameter decorator and checks if the item exists using the **ItemsService**. If the item doesn't exist, a **NotFoundException** is thrown.

By using this approach, you can ensure that your API handles entity not found scenarios elegantly, providing clear and consistent error responses.

In a nutshell, NestJS pipes simplify error handling and enhance your API's robustness by encapsulating validation and error logic.
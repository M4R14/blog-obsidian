NestJS pipes are essential for data validation and transformation. They are used to preprocess incoming data before it reaches your controller methods. In this example, we'll use pipes to validate and transform incoming slug parameters.

Consider the following code snippet:
```ts
// Import required modules
import { Controller, Get, Param } from '@nestjs/common';
import { SlugValidationPipe } from './slug-validation.pipe';
import { Article } from './article.entity';

@Controller('articles')
export class ArticleController {
    @Get(':slug')
    getArticleBySlug(@Param('slug', SlugValidationPipe) article: Article) {
        return article;
    }
}
```
In this code, we define a route parameter named `slug` using the `@Param()` decorator. The `SlugValidationPipe` validates and transforms the slug before passing it to the `getArticleBySlug` method.

Here's how the `SlugValidationPipe` works:
```ts
// SlugValidationPipe.ts
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';
import { ArticleService } from './article.service';

@Injectable()
export class SlugValidationPipe implements PipeTransform {
    constructor(private articleService: ArticleService) {}

    async transform(value: any) {
        const article = await this.articleService.getArticleBySlug(value);

        if (!article) {
            throw new BadRequestException('Article not found');
        }

        return article;
    }
}
```

Inside the `SlugValidationPipe`, customize validation and transformation logic as needed. This example assumes an `ArticleService` with a `getArticleBySlug` method to retrieve articles using slugs.


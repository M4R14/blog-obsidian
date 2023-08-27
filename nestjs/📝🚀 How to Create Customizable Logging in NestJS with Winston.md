Logging is an essential part of any application. It helps you keep track of what's happening under the hood and can help you troubleshoot issues when they arise. In this blog post, we'll show you how to create customizable logging in NestJS with Winston, a popular logging library.

### 🔨 Setting up Winston

To get started, we need to install Winston and NestJS Winston. NestJS Winston is a module that integrates Winston with NestJS, making it easy to use in our application.

```sh
npm install winston nest-winston
```

### 🎨 Creating a Custom Logger Factory

Next, we'll create a logger factory that creates a Winston logger instance with customizable console output. We'll use two different console output formats, one for humans and one for machines, and we'll make it easy to switch between them using an environment variable.

Here's what the `LoggerFactory.ts` file looks like:

```ts
import { transports, format } from 'winston';
import { WinstonModule, utilities as nestWinstonModuleUtilities } from 'nest-winston';

export const LoggerFactory = (appName: string) => {
  let consoleFormat; 

  const DEBUG = process.env.DEBUG
  const USE_JSON_LOGGER = process.env.USE_JSON_LOGGER

  if (USE_JSON_LOGGER === 'true') {
    consoleFormat = format.combine(format.ms(), format.timestamp(), format.json());
  } else {
    consoleFormat = format.combine(
      format.timestamp(),
      format.ms(),
      nestWinstonModuleUtilities.format.nestLike(appName, {
        colors: true,
        prettyPrint: true,
      }),
    );
  }

  return WinstonModule.createLogger({
    level: DEBUG ? 'debug' : 'info',
    transports: [new transports.Console({ format: consoleFormat })],
  });
};
```

We're using two different Winston formats, one for human-readable output and one for JSON output. We're checking an environment variable called `USE_JSON_LOGGER` to determine which format to use. If `USE_JSON_LOGGER` is set to `true`, we'll use the JSON format. Otherwise, we'll use the human-readable format.

### 🚀 Using the Custom Logger Factory

Now that we have our logger factory, we can use it in our NestJS application. We'll replace the default logger in `main.ts` with our custom logger factory.

```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { LoggerFactory } from './common/logger.factory';

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    logger: LoggerFactory('MyApp'),
  });
  await app.listen(3000);
}
bootstrap();
```

Now, when we run our NestJS application, we'll see log output in either the human-readable or JSON format, depending on the value of the `USE_JSON_LOGGER` environment variable.

### 🎉 Conclusion

In this blog post, we've shown you how to create customizable logging in NestJS with Winston. We've created a logger factory that creates a Winston logger instance with customizable console output. We've used two different console output formats, one for humans and one for machines, and we've made it easy to switch between them using an environment variable.

Thanks for reading! We hope you found this tutorial helpful. Happy logging! 🚀
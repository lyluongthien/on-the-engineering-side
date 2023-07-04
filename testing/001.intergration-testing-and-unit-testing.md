# Intergration testing and Unit testing - **/On the engineering side/**

Unit testing and integration testing are two common types of software testing methodologies. Here's a quick comparison of the two:

## Unit testing:
Unit testing is a software testing technique in which individual units of code (such as functions, methods, or classes) are tested separately to ensure they behave as expected. These tests are typically written by developers and run as part of a continuous integration (CI) process to detect problems early in the development cycle.

## Integration testing:
Integration testing is a software testing technique in which multiple units of code are tested together to ensure they work properly together. These tests ensure that the various modules and components of a system work as expected when combined, ensuring that the overall functionality of the application is correct.
## Examples
Now, let's see some examples of how these two testing methodologies can be applied in NestJS.

Unit testing example in NestJS:
Consider the following example of a NestJS service that adds two numbers:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class CalculatorService {
  add(a: number, b: number): number {
    return a + b;
  }
}
```
We can write a unit test for this service's `add()` method using Jest, the default testing library used by NestJS:

```typescript
import { CalculatorService } from './calculator.service';

describe('CalculatorService', () => {
  let service: CalculatorService;

  beforeEach(() => {
    service = new CalculatorService();
  });

  describe('add', () => {
    it('should return the sum of two numbers', () => {
      expect(service.add(2, 3)).toBe(5);
    });
  });
});
```
Here, we're testing the `add()` method of the CalculatorService class in isolation, making sure that it returns the correct output when given two numbers.

Integration testing example in NestJS:
Consider the following example of a NestJS controller that uses the CalculatorService to add two numbers:


```typescript
import { Controller, Get, Param } from '@nestjs/common';
import { CalculatorService } from './calculator.service';

@Controller('calculator')
export class CalculatorController {
  constructor(private readonly calculatorService: CalculatorService) {}

  @Get(':a/:b')
  add(@Param('a') a: number, @Param('b') b: number): number {
    return this.calculatorService.add(a, b);
  }
}

```
To test this controller's `add()` method, we need to make sure that it returns the correct output when given two numbers. However, since the method relies on the CalculatorService, we also need to ensure that the service is working correctly. Here's an integration test that covers both scenarios:


```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { CalculatorController } from './calculator.controller';
import { CalculatorService } from './calculator.service';

describe('CalculatorController', () => {
  let controller: CalculatorController;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [CalculatorController],
      providers: [CalculatorService],
    }).compile();

    controller = module.get<CalculatorController>(CalculatorController);
  });

  describe('add', () => {
    it('should return the sum of two numbers', () => {
      expect(controller.add(2, 3)).toBe(5);
    });
  });
});
```
Here, we're testing the `add()` method of the CalculatorController class, making sure that it returns the correct output when given two numbers. However, because the controller relies on the CalculatorService, we must also ensure that it is operational by including it in the module's.
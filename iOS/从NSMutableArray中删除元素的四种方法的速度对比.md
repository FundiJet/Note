### 测试代码
```objc
    // @[@"1", @"2", @"3", @"4", @"5", @"1", @"2", @"3", @"4", @"5"]
    NSMutableArray *testArr1 = [NSMutableArray arrayWithObjects:@"1", @"2", @"3", @"4", @"5", @"1", @"2", @"3", @"4", @"5", nil];
    NSMutableArray *testArr2 = [NSMutableArray arrayWithObjects:@"1", @"2", @"3", @"4", @"5", @"1", @"2", @"3", @"4", @"5", nil];
    NSMutableArray *testArr3 = [NSMutableArray arrayWithObjects:@"1", @"2", @"3", @"4", @"5", @"1", @"2", @"3", @"4", @"5", nil];
    NSMutableArray *testArr4 = [NSMutableArray arrayWithObjects:@"1", @"2", @"3", @"4", @"5", @"1", @"2", @"3", @"4", @"5", nil];
    
    NSString *string = @"4";
    
    NSTimeInterval begin, end, result1, result2, result3, result4;
    
    // forin
    begin = CACurrentMediaTime();
    for (NSString *item in [testArr1 reverseObjectEnumerator]) {
        if ([item isEqualToString:string]) {
            [testArr1 removeObject:item];
        }
    }
    end = CACurrentMediaTime();
    result1 = (end - begin) * 1000;
    printf("\nMethod 1 time:%5.12f \n", result1);
    printf("--------------------------");
    
    // enumerate block
    begin = CACurrentMediaTime();
    [testArr2 enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(NSString * _Nonnull item, NSUInteger idx, BOOL * _Nonnull stop) {
        if ([item isEqualToString:string]) {
            [testArr2 removeObjectAtIndex:idx];
        }
    }];
    end = CACurrentMediaTime();
    result2 = (end - begin) * 1000;
    printf("\nMethod 2 time:%5.12f \n", result2);
    printf("--------------------------");
    
    // for 循环
    begin = CACurrentMediaTime();
    NSInteger count = [testArr3 count] - 1;
    for (NSInteger i = count; i >= 0; --i) {
        if ([testArr3[i] isEqualToString:string]) {
            [testArr3 removeObjectAtIndex:i];
        }
    }
    end = CACurrentMediaTime();
    result3 = (end - begin) * 1000;
    printf("\nMethod 3 time:%5.12f \n", result3);
    printf("--------------------------");
    
    // while 循环
    begin = CACurrentMediaTime();
    NSInteger number = [testArr4 count] - 1;
    while (number) {
        if ([testArr4[number] isEqualToString:string]) {
            [testArr4 removeObjectAtIndex:number];
        }
        --number;
    }
    end = CACurrentMediaTime();
    result4 = (end - begin) * 1000;
    printf("\nMethod 4 time:%5.12f \n", result4);
    printf("--------------------------");
```
### 单次测试结果
测试结果如下：

```bash
Method 1 time:0.048717000027 
--------------------------
Method 2 time:0.005118999979 
--------------------------
Method 3 time:0.006147000022 
--------------------------
Method 4 time:0.002730999768 
--------------------------
```
### 千次测试结果
- 第 1 种方法最快的次数：**4**
- 第 2 种方法最快的次数：**9**
- 第 3 种方法最快的次数：**45**
- 第 4 种方法最快的次数：**942**

### 万次测试结果
- 第 1 种方法最快的次数：**4**
- 第 2 种方法最快的次数：**70**
- 第 3 种方法最快的次数：**267**
- 第 4 种方法最快的次数：**9658**
- 第 3 种方法与第 4 种方法速度并列第一：**1**

### 速度总结
while 循环 > for 循环 > enumerate block > forin 循环



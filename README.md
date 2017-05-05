# UIView-Maker
A category for UIView to copy and reuse.

## How to copy UIView ? Use NSKeyedArchiver/NSKeyedUnarchiver

Core:

```objective-c
+ (__kindof UIView*)duplicate:(__kindof UIView*)view
{
    NSData * tempArchive = [NSKeyedArchiver archivedDataWithRootObject:view];
    return [NSKeyedUnarchiver unarchiveObjectWithData:tempArchive];
}
```

Than, we can use `- (__kindof UIView*)clone;`to copy UIIView

```objective-c
- (UIView *)clone
{
    return [[self class] duplicate:self];
}
```

## Regist standard UIView as model

We can set key, and use `make block ` to set style of this view.

```objective-c
+ (void)registStyle:(NSString*)key make:(void(^)(id view))makeBlock;
```

for example : 

```objective-c
[MDCButton registStyle:@"category_button" make:^(MDCButton*  _Nonnull view) {
        view.frame = CGRectMake(15, 30, 50, 50);
        [view setBackgroundColor:[UIColor add_colorWithRGBHexString:@"#178EDA"] forState:UIControlStateNormal];
    }];
```

when we need use `category_button` ：

```objective-c
MDCButton* clone = [MDCButton cloneForKey:@"category_button"];
```

## Layout of views with same style

Use `copyTimes`function to layout views with same style : 

```objective-c
- (void)copyTimes:(NSUInteger)times make:(void(^)(id view, NSUInteger idx))makeBlock;
```

for example:

```objective-c
UILabel* label = [[UILabel alloc] init];
    [label copyTimes:10 make:^(id  _Nonnull view, NSUInteger idx) {
      [self.view addSubview:view];
      [view setFrame:CGRectMake(0, idx * 100, 50, 100)];
}];
```


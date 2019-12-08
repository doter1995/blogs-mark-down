## 异常处理的流程
### 默认情况下Spring帮我们的。
当发生异常的时候：
1. spring会去找`/error`对应的controller处理。
2. 再去找`/error`对应的`html,jsp`,以及模板等。
3. 最后才会使用spring提供的默认模板。

### 我们该做什么
对应上面的逻辑，我们该做的是： 在1之前处理异常。
我们应该捕获异常并指定Response
所以我们需要以下两个注解：
#### 1. @ResponseStatus 修改Response状态
首先当程序直接出现抛出异常的话，最终服务器会返回HTTP 500。

那么我们可以自定义要处理的异常，通过`@ResponseStatus`来修改返回的状态。
```
 @ResponseStatus(value=HttpStatus.NOT_FOUND, reason="No such Order")  // 404
 public class OrderNotFoundException extends RuntimeException {
     // ...
 }
```
```
 @RequestMapping(value="/orders/{id}", method=GET)
 public String showOrder(@PathVariable("id") long id, Model model) {
     
     if (order == null) throw new OrderNotFoundException(id);

     return "orderDetail";
 }
```
这种自定义的异常，我们在任意地方抛出。

> 以上方式处理通常需要我们自己去抛出异常。
#### 2. @ExceptionHandler 当有异常时，我们指定返回的内容
以下例子是基于Controller的，什么意思呢？
```
@Controller
public class ExceptionHandlingController {

  // @RequestHandler methods
  // 这里可以写一些处理的请求，
  ...
  
  // Exception handling methods
  
  // 定义HTTP 状态 code
  @ResponseStatus(value=HttpStatus.CONFLICT,
                  reason="Data integrity violation")  // 409
  // 重点：捕获DataIntegrityViolationException的异常
  @ExceptionHandler(DataIntegrityViolationException.class)
  public void conflict() {
    // Nothing to do
  }
  // 返回'databaseError',接下来交由spring mvc处理，比如返回databaseError.jsp
  @ExceptionHandler({SQLException.class,DataAccessException.class})
  public String databaseError() {
    
    return "databaseError";
  }
}
  ```
看代码很容易理解吧，可以捕获任何抛出来的异常并进行转换。
比如SQLException,甚至于Exception。
> 在当前的Controller中使用，估计累死人了，所以接下来

#### 3. 基于全局异常处理 @ControllerAdvice
```
@ControllerAdvice
public class GlobalExceptionHandlingControllerAdvice {
	@ResponseStatus(value = HttpStatus.CONFLICT, reason = "Data integrity violation")
	// 409
	@ExceptionHandler(DataIntegrityViolationException.class)
	public void conflict() {
		logger.error("Request raised a DataIntegrityViolationException");
		// Nothing to do
	}
}
```
也就是将Controller中的`ExceptionHandler`移入到`ControllerAdvice `
这样的话，就可以实现全局的异常处理。
```
// 指定范围 @RestController
@ControllerAdvice(annotations = RestController.class)
public class ExampleAdvice1 {}

// 指定范围  packages
@ControllerAdvice("org.example.controllers")
public class ExampleAdvice2 {}

// 指定范围  classes
@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
public class ExampleAdvice3 {}
```
### 4. @RestControllerAdvice
同3一样，只是专门做Restful的处理。
区别就是Controller和RestController区别
> 到了这里基本常用的处理异常方式已经完了。
### 再深一层的处理方式
#### HandlerExceptionResolver的实现
1. ExceptionHandlerExceptionResolver
将未捕获的异常与@ExceptionHandler处理程序（控制器）和任何控制器建议上的合适方法进行匹配。
2. ResponseStatusExceptionResolver
查找注释的未捕获异常@ResponseStatus（如第1部分所述）
3. DefaultHandlerExceptionResolver
转换标准的Spring异常并将它们转换为HTTP状态代码
4. SimpleMappingExceptionResolver

所以可以考虑去对以上进行扩展。
### 顺着开始的逻辑去修改默认的实现方式。
比如2中的的修改



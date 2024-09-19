---
abbrlink: '0'
---
GuiguException



```
package com.this0.spzx.service.exception;

import com.this0.spzx.model.vo.common.ResultCodeEnum;
import lombok.Data;

/**
 * @Author： YuPengtao
 * @Date： 2024/5/11 下午11:54
 * @Describe：
 */
@Data
public class GuiguException extends RuntimeException {

    private Integer code;          // 错误状态码
    private String message;        // 错误消息

    private ResultCodeEnum resultCodeEnum;     // 封装错误状态码和错误消息

    public GuiguException(ResultCodeEnum resultCodeEnum) {
        this.resultCodeEnum = resultCodeEnum;
        this.code = resultCodeEnum.getCode();
        this.message = resultCodeEnum.getMessage();
    }

    public GuiguException(Integer code, String message) {
        this.code = code;
        this.message = message;
    }

}
```

2 GlobalExceptionHandler

```
package com.this0.spzx.service.handler;

import com.this0.spzx.model.vo.common.Result;
import com.this0.spzx.service.exception.GuiguException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * 统一异常处理类
 */
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    @ResponseBody
    public Result error(Exception e) {
        e.printStackTrace();
        return Result.build(null, 201, e.getMessage());

    }

    @ExceptionHandler(GuiguException.class)
    @ResponseBody
    public Result error(GuiguException e) {
        e.printStackTrace();
        return Result.build(null, e.getResultCodeEnum());
    }

}
```

3 统一结果处理集

```
package com.this0.spzx.model.vo.common;

import io.swagger.v3.oas.annotations.media.Schema;
import lombok.Data;

@Data
@Schema(description = "响应结果实体类")
public class Result<T> {

    //返回码
    @Schema(description = "业务状态码")
    private Integer code;

    //返回消息
    @Schema(description = "响应消息")
    private String message;

    //返回数据
    @Schema(description = "业务数据")
    private T data;

    // 私有化构造
    private Result() {}

    // 返回数据
    public static <T> Result<T> build(T body, Integer code, String message) {
        Result<T> result = new Result<>();
        result.setData(body);
        result.setCode(code);
        result.setMessage(message);
        return result;
    }

    // 通过枚举构造Result对象
    public static <T> Result build(T body , ResultCodeEnum resultCodeEnum) {
        return build(body , resultCodeEnum.getCode() , resultCodeEnum.getMessage()) ;
    }

}

```


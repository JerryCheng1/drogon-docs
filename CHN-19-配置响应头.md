Drogon添加响应头如下，以便解决跨域策略，data size大等杂症

### 添加自定义响应头字段

在控制器函数内callback的返回对象中:

```c++
auto resp = HttpResponse::newHttpJsonResponse(ret);
resp->addHeader("Your Custom Key","Your Custom Value");
```

### 解决跨域问题

控制器对应xxx.cc如下:

```c++
auto setCORSresq(Json::Value ret){
    auto resp = HttpResponse::newHttpJsonResponse(ret);
    resp->addHeader("access-control-allow-credentials","true");
    resp->addHeader("access-control-allow-origin","*");
    resp->setContentTypeCode(CT_TEXT_HTML);
    return resp;
}

void user::login(formDataReq::login &&plogin,
                std::function<void (const HttpResponsePtr &)> &&callback) const {
    LOG_DEBUG<<"User login "<<plogin.username;
    Json::Value ret;
    ret["code"] = 200;
    auto resp = setCORSresq(ret); //CORS策略正确配置函数
    resp->setStatusCode(k200OK);
    callback(resp);
}
```

access-control-allow-origin 可以按需添加URL参数
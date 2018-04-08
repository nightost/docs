  1.配置dev-server允许跨域，解决无法动态加载的问题
```javascript
devServer: {
    ...
    headers: {
        'Access-Control-Allow-Origin': '*'
    }
}
```
# local test


## 后台运行

```bash
docker run -d -p 5002:5000 \
    -v $(pwd)/data:/mnt/data \
    -v $(pwd)/jupyter_sessions:/mnt/jupyter_sessions \
    sandboxed-jupyter-code-exec
```



## 启动测试

```bash
docker run -it -p 5002:5000 \
    -v $(pwd)/data:/mnt/data \
    -v $(pwd)/jupyter_sessions:/mnt/jupyter_sessions \
    sandboxed-jupyter-code-exec /bin/bash

python3.10 -m uvicorn fastapi_jupyter_api:app --host 0.0.0.0 --port 5000
```


## 接口测试

```bash
# new seesion
curl -X POST http://localhost:5002/start_session -F "user_id=user_test"

# 执行代码
curl -X POST http://localhost:5002/execute \
    -H "Content-Type: application/json" \
    -d '{
        "user_id": "user_test",
        "code": "print(\"Hello, World!\")"
    }'

# 执行代码 - 生成图片
curl -X POST http://localhost:5002/execute \
    -H "Content-Type: application/json" \
    -d '{
        "user_id": "user_test",
        "code": "print(\"Hello, World!\")\nplt.plot([1, 2, 3, 4])\nplt.ylabel(\"some numbers\")\nplt.show()"
    }'
```

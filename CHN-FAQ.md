# FAQ

这是常见问题和答案的列表，与一展说明。

## What's drogon's threading model and best prectices?
Drgon 在线程池上运行，当调用 `app().run()` 时，会在该线程池中创建 HTTP 服务器线程和数据库线程。 它是一个基于顺序任务的系统。 因此，建议在可能的情况下始终使用异步 API 或协程。 详见[理解drogon的线程模型](CHN-FAQ-1-线程模型)。

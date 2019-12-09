---
title: 解决PyCharm调试Django时，设置的断点无效
date: 2019-12-07 14:18:00
categories: 编程之道
tags: 
- PyCharm
- VSCode
- Django
- Python
summary: Django默认不会添加这个环境变量，让断点无效不论是对新手还是老兽都会造成极大的困扰。
---

>  `django==3.0`，启动参数：`runserver 0.0.0.0:8000` ，开启DEBUG。

1. **原因**：通过`manage.py runserver`启动项目时，有两种加载reload的方式，见Django 源码：https://github.com/django/django/blob/stable/3.0.x/django/utils/autoreload.py

   ```python
   def run_with_reloader(main_func, *args, **kwargs):
       signal.signal(signal.SIGTERM, lambda *args: sys.exit(0))
       try:
           if os.environ.get(DJANGO_AUTORELOAD_ENV) == 'true':
               reloader = get_reloader()
               logger.info('Watching for file changes with %s', reloader.__class__.__name__)
               start_django(reloader, main_func, *args, **kwargs)
           else:
               exit_code = restart_with_reloader()
               sys.exit(exit_code)
       except KeyboardInterrupt:
           pass
   ```

   其中，当设置环境变量`RUN_MAIN=true`（即文中变量`DJANGO_AUTORELOAD_ENV`)时，使用`threading.Thread`方法新建线程，此时调试器可以追踪；否则使用`subprocess.run`方法新建进程，此时调试器无法追踪。

2. **解决方案**：设置环境变量`RUN_MAIN=true`

3. **吐槽**：Django默认不会添加这个环境变量，让断点无效不论是对新手还是老兽都会造成极大的困扰。


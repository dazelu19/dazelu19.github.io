## Django 探究

## x @method_decorator(csrf_exempt, name='dispatch')
在这个方法之下需要使用class view.

@csrf_exempt也可以在函数的方式下使用post方法


## x tips
1 在pycharm中同一个代码同时在服务器和本地运行：因为服务器没有root权限，很多东西不好安装，所以只单独用它跑模型，本地访问模型API并且做后续处理，为防止代码更新之后重新开启服务，服务器代码部署号之后便断开自动上传的选项，另外在本地重新绑定一个端口运行`python manage.py runserver 0.0.0.0:8001`即可（不使用8000是因为将服务器的端口映射到了本地，再次使用8000会冲突）

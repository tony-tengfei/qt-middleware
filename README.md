# qt-middleware
MiddleWare to support QT

- Container

    - mysql 8
    - redis 6

- Database

    - qt

- Network

    - qt-network

- Start command

  ```shell
  docker-compose up
  ```

## 使用说明

1. 运行qt-middleware中的docker-compose文件部署mysql、redis
2. application run&debug
  1. Run
    1. 先在idea中将项目打包，生成target文件夹，包含 “.jar” 文件
    2. idea打开项目中的docker-compose文件，**检查修改command命令最后的“.jar”文件名与打包后的可执行文件一致**，然后点击services旁边的docker-compose up按钮 '>>'启动项目
    3. 运行一次后Edit Configurations中会自动添加该docker-compose.yml配置，打开Edit Configurations，编辑该配置，点击Modify options -> Attach to -> None 保存
  2. Debug
    1. 配置启动前自动package并启动docker容器
      1. 打开 Run -> Edit Configurations，点击左上角"+"，选择Remote JVM Debug，输入Name
      2. 在配置界面中Use module classpath下拉菜单选择本项目qt
      3. 在配置界面中Before launch 点击"+" ，选择Run Maven Goal，在Command line中输入 clean package保存
      4. 在配置界面中Before launch 点击"+"，选择Launch Docker Before Debug，点击Docker configuration下拉菜单，选择本项目的docker-compose.yml（若没有要先参考Run运行一次docker compose up），会自动填充Service栏为本项目名称，自动填充Custome command命令，修改Custome command命令address=5005改为address=*:5005，保存
      5. 现在可以直接运行debug，会执行mvn clean package，然后启动docker compose，在Docker管理界面选择qt容器，Log窗口中出现Listening for transport dt_socket at address: 5005 Attach debugger 提示，点击Attach debugger跳转到idea debug窗口，项目开始以常规debug模式运行，跟平常一样使用
  3. 注意：
    1. Application docker-compose.yml文件中command启动命令中的jar包名在升级版本号时需要修改，保持与package生成的可执行文件一致
    2. Debug在点击idea debug stop后，还需要在idea docker管理中把对应的app容器关闭，idea debug stop只会关闭remote debug的连接，app还会在docker容器中继续往下执行


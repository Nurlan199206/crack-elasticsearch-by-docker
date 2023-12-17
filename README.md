# crack-elasticsearch-by-docker

Crack elasticsearch 7.x / 8.x by docker

适用于破解elasticsearch 7.x / 8.x的自动脚本

已测试版本
* elasticsearch 8.2.0
* elasticsearch 8.5.0
* elasticsearch 8.4.3
* elasticsearch 8.7.0

Если установка была через apt,yum то на хосте ставим openjdk-17 и

1. запускаем crack.sh
2. сгенерированную папку output через crack.sh перемещаем в /usr/share/elasticsearch/modules/x-pack-core
3. правим файл build_crack_jar.sh
4. перемещаем файл build_crack_jar.sh в /usr/share/elasticsearch/modules/x-pack-core
и запускаем скрипт
5. cp output/x-pack-core-8.4.3.crack.jar /usr/share/elasticsearch/modules/x-pack-core/x-pack-core-8.4.3.jar

```
{
	"license": {
		"uid": "cd5c2258-7422-4f9a-a7c9-cb8d29a25361",
		"type": "platinum",
		"issue_date_in_millis": 1678665600000,
		"expiry_date_in_millis": 3207746200000,
		"max_nodes": 10000,
		"issued_to": "azi",
		"issuer": "Web Form",
		"signature": "AAAAAwAAAA1a8PJsIPdFZHe4WLkDAAABmC9ZN0hjZDBGYnVyRXpCOW5Bb3FjZDAxOWpSbTVoMVZwUzRxVk1PSmkxaktJRVl5MUYvUWh3bHZVUTllbXNPbzBUemtnbWpBbmlWRmRZb25KNFlBR2x0TXc2K2p1Y1VtMG1UQU9TRGZVSGRwaEJGUjE3bXd3LzRqZ05iLzRteWFNekdxRGpIYlFwYkJiNUs0U1hTVlJKNVlXekMrSlVUdFIvV0FNeWdOYnlESDc3MWhlY3hSQmdKSjJ2ZTcvYlBFOHhPQlV3ZHdDQ0tHcG5uOElCaDJ4K1hob29xSG85N0kvTWV3THhlQk9NL01VMFRjNDZpZEVXeUtUMXIyMlIveFpJUkk2WUdveEZaME9XWitGUi9WNTZVQW1FMG1DenhZU0ZmeXlZakVEMjZFT2NvOWxpZGlqVmlHNC8rWVVUYzMwRGVySHpIdURzKzFiRDl4TmM1TUp2VTBOUlJZUlAyV0ZVL2kvVk10L0NsbXNFYVZwT3NSU082dFNNa2prQ0ZsclZ4NTltbU1CVE5lR09Bck93V2J1Y3c9PQAAAQA+fZ30LicFouBamUw0wXUkbOsUP8p1bevJ+JsC4hWsed4ouqqJFipCa0bJJFISWzssU8BpxWQcnNE4WSSbZlSNuxzo2kGUuyE4wWyJyI7kfVpg8dm8POG0ugsIFLfgQISaFxI0MukpmGVyaukQONC9nqKSGgQ7xX2mOrnEC1tRwvBuiJT4aGulM2yMNxOB49DufwfR6w6KVZtpbbC/9BQtRVLl5Vyy/2I8F/il9q+U2J9EdGS4Gt6bW8N2GvZK4rqaPVSTxyh7YNar4IzErpfea9nYkdcgCJ9yOcZw4dCcwaTC90RTYqDIyIQ5h7ET+1Gpr9NemrrbYqfxUR2oIEmX",
		"start_date_in_millis": 1678665600000
	}
}
```

# 注意事项

JDK版本，必须与ES使用的版本完全一致，即使是小版本。
curl拉取github文件，网络不好会导致失败，建议全局代理。

## Usage

Get srcipt

获取脚本源码

```shell
git clone https://github.com/wolfbolin/crack-elasticsearch-by-docker.git
```

Run srcipt with version

运行脚本并指定完整版本（例如: 8.2.0)

```shell
cd crack-elasticsearch-by-docker
version=8.2.0
bash crack.sh $version
```

Get cracked x-pack-core-$version.jar

编译产物和编译中间件保存在output文件夹中

```shell
cp output/x-pack-core-$version.crack.jar x-pack-core-$version.jar
```
### 覆盖路径

RPM包安装默认路径

```shell
/usr/share/elasticsearch/modules/x-pack-core/

```



## Others
You can change `crack.sh` shell with http_proxy / https_proxy url

你可以修改`crack.sh`以使用代理访问网络，避免网络访问故障

```shell
docker build --no-cache -f Dockerfile \
  --build-arg VERSION="${version}" \
  --build-arg HTTP_PROXY="http://1.2.3.4:8080" \
  --build-arg HTTPS_PROXY="http://1.2.3.4:8080" \
  --tag ${service_name}:${version} .

docker run -it --rm \
  -v $(pwd)/output:/crack/output \
  -e HTTP_PROXY="http://1.2.3.4:8080" \
  -e HTTPS_PROXY="http://1.2.3.4:8080" \
  ${service_name}:${version}
```


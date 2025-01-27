---
title: Azure Database for MySQL에 안전하게 연결하기 위한 SSL 연결 구성
description: SSL 연결을 올바르게 사용하기 위해 MySQL용 Azure Database 및 연결된 애플리케이션을 올바르게 구성하는 방법에 대한 지침
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 07/02/2019
ms.openlocfilehash: 46aca2c1a7d40df69b89e15917ff07b983f5ff5f
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561479"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>MySQL용 Azure Database에 안전하게 연결하기 위한 사용자 애플리케이션의 SSL 연결 구성
MySQL용 Azure Database는 SSL(Secure Sockets Layer)을 사용한 MySQL용 Azure Database 서버와 클라이언트 애플리케이션 간 연결을 지원합니다. 데이터베이스 서버와 클라이언트 애플리케이션 간 SSL 연결을 적용하면 서버와 애플리케이션 간 데이터 스트림을 암호화함으로써 “메시지 가로채기(man in the middle)” 공격으로부터 보호할 수 있습니다.

## <a name="step-1-obtain-ssl-certificate"></a>1단계: SSL 인증서 받기
[https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)에서 SSL을 통해 MySQL용 Azure Database 서버와 통신하는 데 필요한 인증서를 다운로드하고 인증서 파일을 로컬 드라이브에 저장합니다(이 자습서에서는 c:\ssl을 예로 사용).
**Microsoft Internet Explorer 및 Microsoft Edge:** 다운로드가 완료된 후 인증서 이름을 BaltimoreCyberTrustRoot.crt.pem으로 변경합니다.

## <a name="step-2-bind-ssl"></a>2단계: SSL 바인딩

특정 프로그래밍 언어 연결 문자열을 참조 하십시오 합니다 [샘플 코드](howto-configure-ssl.md#sample-code) 아래.

### <a name="connecting-to-server-using-mysql-workbench-over-ssl"></a>MySQL Workbench를 사용 하 여 SSL을 통해 서버에 연결
SSL을 통해 안전하게 연결하도록 MySQL Workbench를 구성합니다. 

1. 새 연결 설정 대화 상자에서 **SSL** 탭으로 이동합니다. 

1. 업데이트를 **사용 하 여 SSL** 필드를 "Require"입니다.

1. **SSL CA 파일:** 필드에 BaltimoreCyberTrustRoot.crt.pem의 파일 위치를 입력합니다. 
    
    ![SSL 구성을 저장 합니다.](./media/howto-configure-ssl/mysql-workbench-ssl.png)

기존 연결에 대 한 연결 아이콘을 마우스 오른쪽 단추로 클릭 하 여 SSL을 바인딩하고 수 있으며 편집을 선택할 수 있습니다. 그런 다음 **SSL** 탭으로 이동하고 인증서 파일을 바인딩합니다.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>SSL로 MySQL CLI를 사용하는 서버에 연결
SSL 인증서를 바인딩하는 또 다른 방법은 다음 명령을 실행하여 MySQL 명령줄 인터페이스를 사용하는 것입니다. 

```bash
mysql.exe -h mydemoserver.mysql.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Windows에서 MySQL 명령줄 인터페이스를 사용하는 경우 `SSL connection error: Certificate signature check failed` 오류가 발생할 수 있습니다. 이 오류가 발생하면 `--ssl-mode=REQUIRED --ssl-ca={filepath}` 매개 변수를 `--ssl`로 바꾸세요.

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>3단계:  Azure에 SSL 연결 적용 
### <a name="using-the-azure-portal"></a>Azure Portal 사용
Azure Portal을 사용하여 MySQL용 Azure Database 서버를 방문한 다음 **연결 보안**을 클릭합니다. 설정/해제 단추를 사용하여 **SSL 연결 적용** 설정을 사용하거나 사용하지 않도록 설정한 다음 **저장**을 클릭합니다. Microsoft는 향상된 보안을 위해 항상 **SSL 연결 적용**을 활성화하는 것을 권장합니다.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI 사용
Azure CLI에서 Enabled 또는 Disabled 값을 각각 사용하여 **ssl-enforcement** 매개 변수를 사용하거나 사용하지 않도록 설정할 수 있습니다.
```azurecli-interactive
az mysql server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>4단계: SSL 연결 확인
mysql **status** 명령을 실행하여 SSL로 MySQL 서버에 연결되어 있는지 확인합니다.
```dos
mysql> status
```
출력을 검토하여 연결이 암호화되었는지 확인합니다. 다음이 표시되어야 합니다.  **SSL: Cipher in use is AES256-SHA** 

## <a name="sample-code"></a>샘플 코드
애플리케이션에서 SSL을 통해 Azure Database for MySQL에 대한 안전한 연결을 설정하려면 다음 코드 샘플을 참조하세요.

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="php-using-pdo"></a>PHP (PDO 사용)
```phppdo
$options = array(
    PDO::MYSQL_ATTR_SSL_CA => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
);
$db = new PDO('mysql:host=mydemoserver.mysql.database.azure.com;port=3306;dbname=databasename', 'username@mydemoserver', 'yourpassword', $options);
```
### <a name="python-mysqlconnector-python"></a>Python(MySQLConnector Python)
```python
try:
    conn=mysql.connector.connect(user='myadmin@mydemoserver', 
        password='yourpassword', 
        database='quickstartdb', 
        host='mydemoserver.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```

### <a name="python-pymysql"></a>Python(PyMySQL)
```python
conn = pymysql.connect(user = 'myadmin@mydemoserver', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'mydemoserver.mysql.database.azure.com', 
        ssl = {'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```

### <a name="django-pymysql"></a>Django (PyMySQL)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin@mydemoserver',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ssl-ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}
        }
    }
}
```

### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com',
        :username => 'myadmin@mydemoserver',
        :password => 'yourpassword',
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```

### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')   
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-mysql-connector-for-java"></a>Java (Java 용 MySQL 커넥터)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb-connector-for-java"></a>Java (Java 용 MariaDB 커넥터)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "BaltimoreCyberTrustRoot.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

## <a name="next-steps"></a>다음 단계
[MySQL용 Azure Database에 대한 연결 라이브러리](concepts-connection-libraries.md)에 이어지는 다양한 애플리케이션 연결 옵션 검토

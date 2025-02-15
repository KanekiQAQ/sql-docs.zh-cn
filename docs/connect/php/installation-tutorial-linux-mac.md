---
title: Microsoft Drivers for PHP for SQL Server 的 Linux 和 macOS 安装教程 | Microsoft Docs
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: ulvii
ms.author: v-ulibra
manager: v-mabarw
ms.openlocfilehash: 7a2312a4ff6af5a11825274e3e010873ef2d3bd9
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68256706"
---
# <a name="linux-and-macos-installation-tutorial-for-the-microsoft-drivers-for-php-for-sql-server"></a>Microsoft Drivers for PHP for SQL Server 的 Linux 和 macOS 安装教程
以下说明假定一个干净的环境，并演示如何在 Ubuntu 16.04、18.04 和 18.10、RedHat 7、Debian 8 和 9、Suse 12 和 15 以及 macOS 10.12、10.13 和 10.14 上安装 PHP 7.x、Microsoft ODBC 驱动程序、Apache 和 Microsoft Drivers for PHP for SQL Server。 这些说明建议使用 PECL 安装驱动程序，但也可以从 [Microsoft Drivers for PHP for SQL Server](https://github.com/Microsoft/msphpsql/releases) Github 项目页下载预生成的二进制文件，并按照[下载 Microsoft Drivers for PHP for SQL Server](../../connect/php/loading-the-php-sql-driver.md) 中的说明安装它们。 有关扩展加载以及为什么不将扩展添加到 php.ini 的说明，请参阅[加载驱动程序](../../connect/php/loading-the-php-sql-driver.md##loading-the-driver-at-php-startup)部分。

默认情况下，这些说明安装 PHP 7.3。 注意，一些受支持的 Linux 发行版默认为 PHP 7.0 或更早版本，而 SQL Server 的 PHP 驱动程序不支持这些版本，请参阅每一节开头的说明，以安装 PHP 7.1 或 7.2。

## <a name="contents-of-this-page"></a>此页的内容：

- [在 Ubuntu 16.04、18.04 和 18.10 上安装驱动程序](#installing-the-drivers-on-ubuntu-1604-1804-and-1810)
- [在 Red Hat 7 上安装驱动程序](#installing-the-drivers-on-red-hat-7)
- [在 Red Hat 8 和 9 上安装驱动程序](#installing-the-drivers-on-debian-8-and-9)
- [在 Suse 12 和 15 上安装驱动程序](#installing-the-drivers-on-suse-12-and-15)
- [在 macOS Sierra、High Sierra 和 Mojave 上安装驱动程序](#installing-the-drivers-on-macos-sierra-high-sierra-and-mojave)

## <a name="installing-the-drivers-on-ubuntu-1604-1804-and-1810"></a>在 Ubuntu 16.04、18.04 和 18.10 上安装驱动程序

> [!NOTE]
> 若要安装 PHP 7.1 或 7.2，请使用以下命令将 7.3 替换为 7.1 或 7.2。

### <a name="step-1-install-php"></a>步骤 1. 安装 PHP
```
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php7.3 php7.3-dev php7.3-xml -y --allow-unauthenticated
```
### <a name="step-2-install-prerequisites"></a>步骤 2. 安装先决条件
按照 [Linux 和 macOS 安装页](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)上的说明安装适用于 Ubuntu 的 ODBC 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序
```
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.3/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.3/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.3 sqlsrv pdo_sqlsrv
```

如果系统中只有一个 PHP 版本, 则可以将最后一个步骤简化为`phpenmod sqlsrv pdo_sqlsrv`。

### <a name="step-4-install-apache-and-configure-driver-loading"></a>步骤 4. 安装 Apache 并配置驱动程序加载
```
sudo su
apt-get install libapache2-mod-php7.3 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php7.3
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>步骤 5. 重启 Apache 并测试示例脚本
```
sudo service apache2 restart
```
若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-the-drivers-on-red-hat-7"></a>在 Red Hat 7 上安装驱动程序

> [!NOTE]
> 若要安装 PHP 7.1 或 7.2，请在以下命令中分别用 remi- ph71 或 remi-php72 替换 remi-php73。

### <a name="step-1-install-php"></a>步骤 1. 安装 PHP

```
sudo su
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget https://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum install yum-utils
yum-config-manager --enable remi-php73
yum update
yum install php php-pdo php-xml php-pear php-devel re2c gcc-c++ gcc
```
### <a name="step-2-install-prerequisites"></a>步骤 2. 安装先决条件
按照 [Linux 和 macOS 安装页](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)上的说明安装适用于 Red Hat 7 的 ODBC 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序
```
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```

也可以从 [Github 项目页](https://github.com/Microsoft/msphpsql/releases)下载预生成的二进制文件，或从 Remi 存储库安装：
```
sudo yum install php-sqlsrv
```
### <a name="step-4-install-apache"></a>步骤 4. 安装 Apache
```
sudo yum install httpd
```
默认安装 SELinux，并在强制模式下运行。 若要允许 Apache SELinux 通过连接到数据库，请运行以下命令：
```
sudo setsebool -P httpd_can_network_connect_db 1
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>步骤 5. 重启 Apache 并测试示例脚本
```
sudo apachectl restart
```
若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-the-drivers-on-debian-8-and-9"></a>在 Debian 8 和 9 上安装驱动程序

> [!NOTE]
> 若要安装 PHP 7.1 或 7.2，请使用以下命令将 7.3 替换为 7.1 或 7.2。

### <a name="step-1-install-php"></a>步骤 1. 安装 PHP
```
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php7.3 php7.3-dev php7.3-xml
```
### <a name="step-2-install-prerequisites"></a>步骤 2. 安装先决条件
按照 [Linux 和 macOS 安装页](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)上的说明安装适用于 Debian 的 ODBC 驱动程序。 

可能还需要生成正确的区域设置，以使 PHP 输出在浏览器中正确显示。 例如，对于 en_US UTF-8 区域设置，运行以下命令：
```
sudo su
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序
```
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.3/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.3/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 7.3 sqlsrv pdo_sqlsrv
```

如果系统中只有一个 PHP 版本, 则可以将最后一个步骤简化为`phpenmod sqlsrv pdo_sqlsrv`。

### <a name="step-4-install-apache-and-configure-driver-loading"></a>步骤 4. 安装 Apache 并配置驱动程序加载
```
sudo su
apt-get install libapache2-mod-php7.3 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php7.3
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>步骤 5. 重启 Apache 并测试示例脚本
```
sudo service apache2 restart
```
若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-the-drivers-on-suse-12-and-15"></a>在 Suse 12 和 15 上安装驱动程序

> [!NOTE]
> 在下面的说明中，将 <SuseVersion> 替换为 Suse 版本，如果使用的是 Suse Enterprise Linux 15，它将是 SLE_15 或 SLE_15_SP1，其他版本也是如此。 并不是所有版本的 PHP 都适用于所有版本的 Suse Linux，请参阅 `http://download.opensuse.org/repositories/devel:/languages:/php` 以查看哪些版本的 Suse 具有默认版本的 PHP 可用，或者参阅 `http://download.opensuse.org/repositories/devel:/languages:/php:/` 查看哪些其他版本的 PHP 可用于哪些版本的 Suse。

> [!NOTE]
> PHP 7.3 的包不可用于 Suse 12。 若要安装 PHP 7.1，用以下 URL 替换下面的存储库 URL：`https://download.opensuse.org/repositories/devel:/languages:/php:/php71/<SuseVersion>/devel:languages:php:php71.repo`。
> 若要安装 PHP 7.2，用以下 URL 替换下面的存储库 URL：`https://download.opensuse.org/repositories/devel:/languages:/php:/php72/<SuseVersion>/devel:languages:php:php72.repo`。

### <a name="step-1-install-php"></a>步骤 1. 安装 PHP
```
sudo su
zypper -n ar -f https://download.opensuse.org/repositories/devel:languages:php/<SuseVersion>/devel:languages:php.repo
zypper --gpg-auto-import-keys refresh
zypper -n install php7 php7-pear php7-devel php7-openssl
```
### <a name="step-2-install-prerequisites"></a>步骤 2. 安装先决条件
按照 [Linux 和 macOS 安装页](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)上的说明安装适用于 Suse 的 ODBC 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序
> [!NOTE]
> 如果收到错误消息 `Connection to 'pecl.php.net:443' failed: Unable to find the socket transport "ssl"`，请编辑 /usr/bin/pecl 中的 pecl 脚本并删除最后一行的 `-n` 开关。 此开关可防止 PECL 在调用 PHP 时加载 ini 文件，从而防止加载 OpenSSL 扩展。

```
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>步骤 4. 安装 Apache 并配置驱动程序加载
```
sudo su
zypper install apache2 apache2-mod_php7
a2enmod php7
echo "extension=sqlsrv.so" >> /etc/php7/apache2/php.ini
echo "extension=pdo_sqlsrv.so" >> /etc/php7/apache2/php.ini
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>步骤 5. 重启 Apache 并测试示例脚本
```
sudo systemctl restart apache2
```
若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-the-drivers-on-macos-sierra-high-sierra-and-mojave"></a>在 macOS Sierra、High Sierra 和 Mojave 上安装驱动程序

如果还没有安装 brew，请按照以下步骤安装：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> [!NOTE]
> 若要安装 PHP 7.1 或 7.2，请在以下命令中分别用 php@7.1 或 php@7.2 替换 php@7.3。

### <a name="step-1-install-php"></a>步骤 1. 安装 PHP

```
brew tap
brew tap homebrew/core
brew install php@7.3
```
PHP 现应在路径中 - 运行 `php -v` 以验证正在运行正确版本的 PHP。 如果 PHP 不在路径中或版本不正确，则运行以下命令：
```
brew link --force --overwrite php@7.3
```

### <a name="step-2-install-prerequisites"></a>步骤 2. 安装先决条件
按照 [Linux 和 macOS 安装页](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)上的说明安装适用于 macOS 的 ODBC 驱动程序。 

此外，可能需要安装 GNU make 工具：
```
brew install autoconf automake libtool
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序
```
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>步骤 4. 安装 Apache 并配置驱动程序加载
```
brew install apache2
```
若要查找用于 Apache 安装的 Apache 配置文件，请运行 
```
apachectl -V | grep SERVER_CONFIG_FILE
``` 
并在以下命令中用路径替换 `httpd.conf`：
```
echo "LoadModule php7_module /usr/local/opt/php@7.3/lib/httpd/modules/libphp7.so" >> /usr/local/etc/httpd/httpd.conf
(echo "<FilesMatch .php$>"; echo "SetHandler application/x-httpd-php"; echo "</FilesMatch>";) >> /usr/local/etc/httpd/httpd.conf
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>步骤 5. 重启 Apache 并测试示例脚本
```
sudo apachectl restart
```
若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="testing-your-installation"></a>测试安装

若要测试此示例脚本，请在系统的文档根目录中创建名为“testsql.php”的文件。 这在 Ubuntu、 Debian 和 Redhat 上为 `/var/www/html/`，在 SUSE 上为 `/srv/www/htdocs`，在 macOS 上为 `/usr/local/var/www`。 将以下脚本复制到其中，酌情替换服务器、数据库、用户名和密码。
```
<?php
$serverName = "yourServername";
$connectionOptions = array(
    "database" => "yourDatabase",
    "uid" => "yourUsername",
    "pwd" => "yourPassword"
);

// Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if ($conn === false) {
    die(formatErrors(sqlsrv_errors()));
}

// Select Query
$tsql = "SELECT @@Version AS SQL_VERSION";

// Executes the query
$stmt = sqlsrv_query($conn, $tsql);

// Error handling
if ($stmt === false) {
    die(formatErrors(sqlsrv_errors()));
}
?>

<h1> Results : </h1>

<?php
while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
    echo $row['SQL_VERSION'] . PHP_EOL;
}

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

function formatErrors($errors)
{
    // Display errors
    echo "Error information: <br/>";
    foreach ($errors as $error) {
        echo "SQLSTATE: ". $error['SQLSTATE'] . "<br/>";
        echo "Code: ". $error['code'] . "<br/>";
        echo "Message: ". $error['message'] . "<br/>";
    }
}
?>
```
将浏览器指向 https://localhost/testsql.php （macOS 上的 https://localhost:8080/testsql.php ）。 现在应能够连接到 SQL Server/Azure SQL 数据库。

## <a name="see-also"></a>另请参阅  
[开始使用 Microsoft Drivers for PHP for SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[加载 Microsoft Drivers for PHP for SQL Server](../../connect/php/loading-the-php-sql-driver.md)

[Microsoft Drivers for PHP for SQL Server 系统要求](../../connect/php/system-requirements-for-the-php-sql-driver.md)

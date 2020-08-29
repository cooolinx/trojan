# trojan

An unidentifiable mechanism that helps you bypass GFW.

Trojan features multiple protocols over `TLS` to avoid both active/passive detections and ISP `QoS` limitations.

Trojan is not a fixed program or protocol. It's an idea, an idea that imitating the most common service, to an extent that it behaves identically, could help you get across the Great FireWall permanently, without being identified ever. We are the GreatER Fire; we ship Trojan Horses.

## Enhanced Trojan

### Access

Record visit details, such as client IP address and the last visit time of visitor, after each connection finishing (as the same timing as recording download/upload traffic usage) to the database. Access records will remove automatically after expiration (expiration is default 10min and configured in MySQL scheme).

This is useful for detecting who is online and how many IP is used by each user.

The scheme of `access` table:

```sql
CREATE TABLE access (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    password VARCHAR(64) NOT NULL,
    address VARCHAR(64) NOT NULL,
    time DATETIME NOT NULL,
    PRIMARY KEY (id),
    UNIQUE KEY `password_address` (password, address)
);
CREATE EVENT expire_access
ON SCHEDULE EVERY 1 MINUTE
DO
    DELETE FROM access
    WHERE TIMESTAMPDIFF(SECOND, time, NOW()) > 600;
SET GLOBAL event_scheduler = ON;
```

## Documentations

The original project is [trojan-gfw/trojan](https://github.com/trojan-gfw/trojan).
An online documentation can be found [here](https://trojan-gfw.github.io/trojan/).  
Installation guide on various platforms can be found in the [wiki](https://github.com/trojan-gfw/trojan/wiki/Binary-&-Package-Distributions).

## Dependencies

- [CMake](https://cmake.org/) >= 3.7.2
- [Boost](http://www.boost.org/) >= 1.66.0
- [OpenSSL](https://www.openssl.org/) >= 1.1.0
- [libmysqlclient](https://dev.mysql.com/downloads/connector/c/)

## License

[GPLv3](LICENSE)

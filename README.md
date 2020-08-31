# trojan

An unidentifiable mechanism that helps you bypass GFW.

Trojan features multiple protocols over `TLS` to avoid both active/passive detections and ISP `QoS` limitations.

Trojan is not a fixed program or protocol. It's an idea, an idea that imitating the most common service, to an extent that it behaves identically, could help you get across the Great FireWall permanently, without being identified ever. We are the GreatER Fire; we ship Trojan Horses.

## Enhanced Trojan

### Access

Saving visit record hourly, including client IP address and the last visit time of visitor (in a nature hour). Saving time is after each connection finishing (as the same timing as recording download/upload traffic usage).

This feature is useful for detecting who is online and how many IP is used by each user.

The scheme of `access` table:

```sql
CREATE TABLE access (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    password VARCHAR(64) NOT NULL,
    address VARCHAR(64) NOT NULL,
    time DATETIME NOT NULL,
    download BIGINT UNSIGNED NOT NULL DEFAULT 0,
    upload BIGINT UNSIGNED NOT NULL DEFAULT 0,
    PRIMARY KEY (id),
    UNIQUE KEY `password_address_time` (password, address, time)
);
```

Notice that, saving visit record hourly may leave a huge number of rows in the table, eg. 10000 users x 24 hour x 365 days = 87,600,000 rows per year, take care of it. Here is a way to clear yesterday records at 4:00 everyday:

```sql
CREATE EVENT expire_access
ON SCHEDULE
    EVERY 1 DAY
    STARTS CURRENT_DATE + INTERVAL 1 DAY + INTERVAL 4 HOUR
    COMMENT 'clear yesterday access records at 4:00 daily'
DO
    DELETE FROM access
    WHERE time < CURRENT_DATE;
SET GLOBAL event_scheduler = ON;
```

## Documentations

- The original project is [trojan-gfw/trojan](https://github.com/trojan-gfw/trojan).
- An online documentation can be found [here](https://trojan-gfw.github.io/trojan/).
- Installation guide on various platforms can be found in the [wiki](https://github.com/trojan-gfw/trojan/wiki/Binary-&-Package-Distributions).

## Dependencies

- [CMake](https://cmake.org/) >= 3.7.2
- [Boost](http://www.boost.org/) >= 1.66.0
- [OpenSSL](https://www.openssl.org/) >= 1.1.0
- [libmysqlclient](https://dev.mysql.com/downloads/connector/c/)

## License

[GPLv3](LICENSE)

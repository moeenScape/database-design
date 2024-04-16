### Database Design
#### ERD
![ERD](youtube_database.png)

```mysql
CREATE TABLE user_profile
(
    id         BIGINT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(255)        NOT NULL,
    last_name  VARCHAR(255)        NOT NULL,
    email      VARCHAR(255) UNIQUE NOT NULL,
    gender     ENUM ('MALE', 'FEMALE') CHECK (gender IN ('MALE', 'FEMALE')),
    created_at TIMESTAMP           NOT NULL
);

create table if not exists youtube_account
(
    user_profile_id BIGINT PRIMARY KEY REFERENCES user_profile (id),
    created_at      TIMESTAMP NOT NULL
);

CREATE TABLE IF NOT EXISTS youtube_channel
(
    id                 BIGINT AUTO_INCREMENT PRIMARY KEY,
    youtube_account_id BIGINT              NOT NULL REFERENCES youtube_account (user_profile_id),
    channel_name       VARCHAR(255) UNIQUE NOT NULL,
    created_at         TIMESTAMP           NOT NULL
);

CREATE TABLE IF NOT EXISTS channel_subscriber
(
    youtube_account_id BIGINT REFERENCES youtube_account (user_profile_id),
    youtube_channel_id BIGINT REFERENCES youtube_channel (id),
    created_at         TIMESTAMP NOT NULL,
    PRIMARY KEY (youtube_account_id, youtube_channel_id)
);

CREATE TABLE IF NOT EXISTS video
(
    id                 BIGINT PRIMARY KEY AUTO_INCREMENT,
    youtube_channel_id BIGINT REFERENCES youtube_channel (id),
    title              VARCHAR(250)        NOT NULL,
    tag                VARCHAR(255)        NULL,
    description        VARCHAR(1000)       NOT NULL,
    url                VARCHAR(300) UNIQUE NOT NULL,
    created_at         TIMESTAMP           NOT NULL
);

CREATE TABLE IF NOT EXISTS video_view
(
    id                 BIGINT PRIMARY KEY AUTO_INCREMENT,
    youtube_account_id BIGINT REFERENCES youtube_account (user_profile_id) NULL,
    video_id           BIGINT REFERENCES video (id),
    created_at         TIMESTAMP                                           NOT NULL
);

CREATE TABLE IF NOT EXISTS video_like
(
    id                 BIGINT PRIMARY KEY AUTO_INCREMENT,
    youtube_account_id BIGINT REFERENCES youtube_account (user_profile_id) NULL,
    video_id           BIGINT REFERENCES video (id),
    created_at         TIMESTAMP                                           NOT NULL
);

CREATE TABLE IF NOT EXISTS video_comment
(
    id                 BIGINT PRIMARY KEY AUTO_INCREMENT,
    youtube_account_id BIGINT REFERENCES youtube_account (user_profile_id) NULL,
    video_id           BIGINT REFERENCES video (id),
    created_at         TIMESTAMP                                           NOT NULL
);
```

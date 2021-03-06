[![](https://jitpack.io/v/EugeneLesnov/spring-boot-starter-rest.svg)](https://jitpack.io/#EugeneLesnov/spring-boot-starter-rest)
[![Build Status](https://travis-ci.com/EugeneLesnov/spring-boot-starter-rest.svg?branch=master)](https://travis-ci.com/EugeneLesnov/spring-boot-starter-rest)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)

# spring-boot-starter-rest
Spring Boot starter provides fast SSL RestTemplate setup for your backend 

## How to use

1. Add to your build.gradle:
```groovy
allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}
```

2. Add dependency (_do not forget to change the tag_):
``` groovy
dependencies {
    implementation 'com.github.EugeneLesnov:spring-boot-starter-rest:Tag'
}
```

3. Provide required SSL properties:
``` yaml
server:
    ssl:
        enabled: true
        key-store: /path/to/key.keystore
        key-store-password: password
        key-alias: alias
        trust-store: /path/to/truststore
        trust-store-password: password
        connection-default-max-per-route: 40 # optional property, defaults to 25
```

4. Additionally, you can implement HostnameVerifier interface and make it a bean. 
Otherwise, starter uses NoopHostnameVerifier instance (verification is off).

Here's a short example:
``` java
public class CustomHostnameVerifier implements HostnameVerifier {

    public CustomHostnameVerifier() {}

    @Override
    public boolean verify(String hostname, SSLSession session) {
        // do your verify stuff
        return false;
    }
}
```

Then:
``` java
@Bean
public HostnameVerifier hostnameVerifier() {
    return new CustomHostnameVerifier();
}
```

## How to generate self-signed certificate with _keytool_ ?

```
1. keytool -genkey -keystore key.keystore -alias certificateAlias -keyalg RSA -keysize 2048 -validity 3950

2. keytool -selfcert -alias certificateAlias -keystore key.keystore -validity 3950

3. keytool -export -alias certificateAlias -keystore key.keystore -rfc -file certificateAlias.cer

4. keytool -importcert -alias certificateAlias -file certificateAlias.cer -keystore truststore
```
---
description: >-
  This page contains docker SAST rules including Vulnerability, Bug, Code Smell,
  Security Hotspot
---

# 🐳 Docker

## <mark style="color:purple;">Code Smell</mark>&#x20;

### 1. Descriptive Labels are Mandatory&#x20;

Issue : when one of the mandatory label are missing . Labels help to organise  images by project, record licensing, aid in the automation  and for other reasons.&#x20;

```docker
// Non -compliant solution 


From Ububtu:22.02
RUN my_command
```

```docker
// Compliant Solution 

From Ubuntu:22.02
LABEL maintainer="shubhendu"
LABEL description=" Image is for testing"
LABEL version=1.0
RUN my_command 
```

2. Argument in long RUN instructions should be sorted&#x20;

in Docker file , commands within RUN argument should be sorted alphabetically if order is not enforced by the command.&#x20;

This practice enhance the readability of the code, easier to track modification & prevent potential errors.&#x20;

```docker
// Non-compliant Solution 

FROM ubuntu:20.04

RUN apt-get update && apt-get install -y \
    unzip \
    wget \
    curl \
    git \
    zip
```

```docker
// Non-compliant Solution 

FROM alpine:3.12

RUN apk add unzip wget curl git zip
```

Here commands are not in alphabetically so let's see compliant solution&#x20;

```docker
// Compliant Solution 

FROM ubuntu:20.04

RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    wget \
    zip
```

```docker
// Compliant Solution 

FROM alpine:3.12

RUN apk add curl git unzip wget zip
```



## <mark style="color:purple;">Security-Hotspot</mark>&#x20;

1. Delivering code in production with debug ft activated is Security-sensitive&#x20;

Why ?&#x20;

Debug instructions or error messages can leak detailed information about the system like application's path or file name&#x20;



Questions to be asked before deploying application to the end users  and if any of the question has "YES" answer then there is a potential Risk&#x20;

{% tabs %}
{% tab title="First Question" %}
The code or configuration enabling the application debug features is deployed on production servers or distributed to end users
{% endtab %}

{% tab title="Second Question" %}
The application runs by default with debug features activated&#x20;
{% endtab %}
{% endtabs %}

```docker
// Non-compliant solution 

FROM example
# Sensitive
ENV APP_DEBUG=true
# Sensitive
ENV ENV=development
CMD /run.sh

```

```docker
// Compliant Solution 

FROM example
ENV APP_DEBUG=false
ENV ENV=production
CMD /run.sh
```

2. Running container as privileged user is security-sensitive&#x20;

\
Running containers as a privileged user weakens their runtime security, allowing any user whose code runs on the container to perform administrative actions.\
In Linux containers, the privileged user is usually named `root`. In Windows containers, the equivalent is `ContainerAdministrator`

<mark style="color:yellow;">`Questions to be Asked`</mark>

{% tabs %}
{% tab title="First Question" %}
Servers services accessible from the Internet&#x20;
{% endtab %}

{% tab title="Second Question" %}
Doesn't require a privileged user to run&#x20;
{% endtab %}
{% endtabs %}

and there is a security risk if any of the Answer is "YES"

<mark style="color:yellow;">**Solution**</mark> :-&#x20;


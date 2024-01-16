# Section-4 Quick introduction to Microservices
# 60. Intro Microservices with Spring Cloud
Why are microservices needed?  
What are the challenges associate with them?  
How does spring cloud help us to solve them.
--
# 61 Step-1 Intro to microservices
Fuzzy - Difficult to understand

### Defination
![Alt text](image-1.png)

### Long Defination
![Alt text](image-2.png)
![Alt text](image-3.png)

### Understand microservice
![Alt text](image-4.png)

1. Services which are exposed by Rest
2. Small deployable unit with very well thought out boundaries
3. this should be cloud enalbed

***Imp Keyword***
1. Rest
2. Small deployable units
3. cloud enabled

***2nd Point : Small deployable units***
![Alt text](image-5.png)

***3rd point:  cloud enaled***
![Alt text](image-6.png)

cloud enabled bole to kal microservice 3 par jayda load aaya to easily uke instance badha sake. 

hum easily instance add kar sake previous wale delete kar sake. without having problem This is called cloud enabled.

---
# 62  Step-2 challenges with microservices
![Alt text](image-7.png)
![Alt text](image-8.png)

![Alt text](image-9.png)

![Alt text](image-10.png)

![Alt text](image-11.png)

# 63 Step-3 Intro to Spring Cloud
Distributed system mein common problem ko address karta Spring cloud.

In umbrella of Spring cloud there are multiple project which address to the problem.

Like   
1 .  Spring Cloud Config project resloved configuration problem  
2. Spring Cloud bus Project resolved communication problem.  
3 . Spring Cloud Netflix resolves problem of Eureka Server.

![Alt text](image-12.png)

Spring cloud Config server provide an approach jaha aap sare configuration ek jagah git mein store kar sakte.(centralize location )

Spring cloud config server ye expose karenga microservices ko different environment ke hisab se.

![Alt text](image-13.png)

![Alt text](image-14.png)

### Feign
ye rest client banane ke kaam ata

![Alt text](image-15.png)

![Alt text](image-16.png)

# 64 Step-04 Advantages of microservice Architecture.
![Alt text](image-17.png)

![Alt text](image-18.png)

# 65 Step-05 Microservices Components  - Standardizing Ports and Url

![Alt text](image-19.png)

![Alt text](image-20.png)

# Section-6 : Microservices with Spring Cloud-V2
# 123 What's new in v2?
![Alt text](image-21.png)

# 125 Have you already completed v1?
![Alt text](image-22.png)

# 127 step-1 Setting up Limits Microservices V2
![Alt text](image-23.png)

***Let's create a limit microservice***

hume limits-service connect karna hai to Spring Cloud Config server. Therefore here in dependency we add Config Client.

![Alt text](image-24.png)

***Note: Don't have any spaces in folder path other wise microservice have problem***

![Alt text](image-25.png)

![Alt text](image-26.png)

# 129 Step-2 a)Creating a hardcoded limits-service V2
***Target:   
a) Create a Rest-Api which returning hardcoded data,   
 b) enhance it to pick value from configuration and  
  c) then from centralized configuration***

***a) Create a Rest-Api which returning hardcoded data***

***Limits.java***
```java
package com.adi.microservices.bean;

public class Limits {

	private int minimum;
	private int maximum;

	public Limits() {
		super();
		// TODO Auto-generated constructor stub
	}

	public Limits(int minimum, int maximum) {
		super();
		this.minimum = minimum;
		this.maximum = maximum;
	}

	public int getMinimum() {
		return minimum;
	}

	public void setMinimum(int minimum) {
		this.minimum = minimum;
	}

	public int getMaximum() {
		return maximum;
	}

	public void setMaximum(int maximum) {
		this.maximum = maximum;
	}
}
```
***LimitsController.java***
```java
package com.adi.microservices.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.adi.microservices.bean.Limits;

@RestController
public class LimitsController {

	@GetMapping("/limits")
	public Limits retriveLimitsBean() {
		
		return new Limits(1,1000);
	}
}
```
***application.properties***
```properties

spring.config.import=optional:configserver:http://localhost:8888

```

![Alt text](image-27.png)

![Alt text](image-28.png)

# 130 Step-3 b) Enhance limits-service - Get configuration from application.props
 ***1. First we set the value in application.properties***  

```properties
spring.config.import=optional:configserver:http://localhost:8888

limits-service.minimum=2
limits-service.maximum=998 

```

***2.  Pick up values from application.properties file. For this Create a Configuration class and fetch everything via ConfigProperties annotation***

```java
package com.adi.microservices.configuration;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

//Give proper name of Configuration Properties since in our application there are many.
@Component
@ConfigurationProperties("limits-service")
public class Configuration {

	private int minimum;
	private int maximum;

	public int getMinimum() {
		return minimum;
	}

	public void setMinimum(int minimum) {
		this.minimum = minimum;
	}

	public int getMaximum() {
		return maximum;
	}

	public void setMaximum(int maximum) {
		this.maximum = maximum;
	}
}
```

***3. Now controller part***
```java
package com.adi.microservices.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.adi.microservices.bean.Limits;
import com.adi.microservices.configuration.Configuration;

@RestController
public class LimitsController {
	
	@Autowired
	private Configuration configuration;

	@GetMapping("/limits")
	public Limits retriveLimitsBean() {
		
		return new Limits(configuration.getMinimum(),configuration.getMaximum());
	}
}
```

![Alt text](image-29.png)

# 134 Step-4 Setting up Spring Cloud Config Server.-v2
Make sure use same version of Spring boot For config server as where config client uses.
![Alt text](image-30.png)

### For config server we use port 8888

***Also give proper name to every project in     application.properties file***

```properties

spring.application.name=spring-cloud-config-server

server.port=8888

```
# 132 Step-5 Installing Git and creating a local git repository
![Alt text](image-31.png)

![Alt text](image-32.png)

![Alt text](image-33.png)

### Go to particular folder where you want to create a git repo
![Alt text](image-34.png)

## Create a proper folder
![Alt text](image-35.png)

### cd to particular folder
![Alt text](image-36.png)

## For present working directory use command
![Alt text](image-37.png)

![Alt text](image-38.png)

## Create a git repository via git init
![Alt text](image-39.png)

Now here we store our centralize configuration, use proper editor for that.

Here we use Visual studio and open that folder in that.

### Here we create limits-service.properties file
![Alt text](image-40.png)

### Now we commit changes
1. Go to cmd and type dir or ls
![Alt text](image-41.png)

2. Now first add them commit via proper commands

for comments (git commit -m "comments")
![Alt text](image-42.png)

# 134 step-6 c) Connect Spring cloud Config Server to local Git Repository -v2

### Connect github repository with config server
![Alt text](image-43.png)
```properties

spring.application.name=spring-cloud-config-server

server.port=8888

spring.cloud.config.server.git.uri=file:///C:/smart_study/microservices_Ranga/git/git-localconfig-repository
```


### Enable Config server
```java
package com.adi.microservices;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@EnableConfigServer
@SpringBootApplication
public class SpringCloudConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringCloudConfigServerApplication.class, args);
	}

}

```

![Alt text](image-44.png)

![Alt text](image-45.png)

# 135 step-7 Connect Limits Service to Spring Cloud Config Server.- V2

## Connect config client to config server
![Alt text](image-46.png)

![Alt text](image-47.png)

## So value configure in git hub
![Alt text](image-48.png)

![Alt text](image-49.png)

![Alt text](image-50.png)

![Alt text](image-51.png)




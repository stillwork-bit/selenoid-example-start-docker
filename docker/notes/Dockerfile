подложить сертификаты
FROM maven:3.6.3-jdk11-with-dependency AS build
WORKDIR /app
#COPY certificates/nexus-cicd.cer .
#RUN keytool -cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias nexus -file nexus-cicd.cer
#COPY . .
#RUN mkdir -p /root/.m2
#COPY ./settings.xml /root/.m2
RUN mvn clean install -DskipTest

FROM openjdk:11-jre
WORKDIR /app
#ENV OPTS = ""
#JAVA_OPTS= "--spring.config.location", "dev.properties"
COPY --from=build /app/target/si-console-dev.jar /app/dev.jar
COPY --from=build /app/src/test/resources/application.properties /app/dev.properties
COPY --from=build /app/src/test/resources/dbkeystore.p12 /app/dbkeystore.p12
#ENTRYPOINT ["java", "$JAVA_OPTS", "-jar", "dev.jar"]
ENTRYPOINT exec java $OPTS_DEV -jar dev.jar


Сбор образа с зависимостями для интеграционных тестов и чисткой на основе образа spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk11-with-dependency

FROM spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk11-with-dependency
#WORKDIR /app
COPY . /app
COPY ./settings.xml /root/.m2
#ENTRYPOINT ["/bin/sh", "start_test.sh"]
RUN mvn clean install -DskipTests
RUN pwd
RUN rm -rf /app/certificates /app/src /app/target /app/test-result /app/.allure
RUN rm -f /app/Dockerfile /app/integration-tests.iml /app/pom.xml /app/readme.md /app/settings.xml /app/start_test.sh


Сбор образа с зависимостями для интеграционных тестов и чисткой на основе образа maven:3.6.3-jdk-11

	FROM spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk-11
	WORKDIR /app
	COPY . .
	RUN keytool -cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias nexus -file ./certificates/nexus-cicd.cer
	RUN mkdir -p /root/.m2
	COPY ./settings.xml /root/.m2
	RUN mvn clean install test -Dgroups=UserManager
	RUN rm -rf /app/certificates /app/src /app/target /app/test-result /app/.allure
	RUN rm -f /app/Dockerfile /app/integration-tests.iml /app/pom.xml /app/readme.md /app/settings.xml /app/start_test.sh

	сборка образа:
	sudo docker build -t spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk11-with-dependency-1.2 .

Сборка образа с тестами

	FROM spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk11-with-dependency-1.2
	WORKDIR /app
	COPY . .
	ENTRYPOINT ["/bin/sh", "start_test.sh"]

	sudo docker build -t integration_tests .

	Проверка образа:
	sudo docker run  --rm -e test_group='UserManager' -e URL='DevUrl.conf' localhost/integration-tests:latest
		или 
	sudo docker run  --rm --add-host stspma-apc003lk.corp.dev.vtb:10.226.35.153 --add-host stspma-apc002lk.corp.dev.vtb:10.226.35.130 --add-host stspma-apc001lk.corp.dev.vtb:10.226.35.140 -e test_group='UserManager' -e URL='DevUrl.conf' localhost/integration_tests:latest
	

            test_plan_name=\"%env.test_plan_name%\""	
	docker run  --rm --add-host stspma-apc003lk.corp.dev.vtb:10.226.35.153 --add-host stspma-apc002lk.corp.dev.vtb:10.226.35.130 --add-host stspma-apc001lk.corp.dev.vtb:10.226.35.140 -e test_group='SmokeUI' -e URL='DevUrl.conf' localhost/integration_tests:latest
	
	
	sudo docker run  --rm --add-host stspma-apc003lk.corp.dev.vtb:10.226.35.153 --add-host stspma-apc002lk.corp.dev.vtb:10.226.35.130 --add-host stspma-apc001lk.corp.dev.vtb:10.226.35.140 --add-host k4-spm-app.vtb24.ru:10.74.210.252 --add-host k4-spm-app2.vtb24.ru:10.74.209.5 --add-host k4-spm-app101lv.vtb24.ru:10.74.177.132 --add-host k3-spm-app.vtb24.ru:10.74.210.250 --add-host k3-spm-app2.vtb24.ru:10.74.209.210 --add-host k3-spm-app101lv.vtb24.ru:10.74.177.221 --add-host ifspma-ap5901ln.test.vtb.ru:10.223.20.152 --add-host rrspma-ap5901ln.test.vtb.ru:10.223.20.141 --add-host testit.corp.dev.vtb:10.185.156.69 --add-host testit.region.vtb:10.185.188.22 localhost/integration_tests:latest
	
	wsl ansible-playbook --vault-id vault_password@vault-password -i integration-tests-inventory-dev.yml integration-tests.yml

docker run --rm  --add-host stspma-apc003lk.corp.dev.vtb:10.226.35.153 --add-host stspma-apc002lk.corp.dev.vtb:10.226.35.130 --add-host stspma-apc001lk.corp.dev.vtb:10.226.35.140 --add-host testit.corp.dev.vtb:10.185.156.69 localhost/integration_tests:latest



ansible-playbook --vault-id vault_password@vault-password -i integration-tests-inventory-dev.yml -e test_group='SmokeUI' -e URL='DevUrl.conf' integration-tests.yml	
	
FROM spma-docker-lib.nexus-ci.corp.dev.vtb/maven:3.6.3-jdk-11
WORKDIR /app
COPY . .
RUN keytool -cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias nexus -file ./certificates/nexus-cicd.cer
RUN mkdir -p /root/.m2
COPY ./settings.xml /root/.m2

RUN mvn clean -Dsurefire.rerunFailingTestsCount=2 test -Dgroups=SmokeUI -Dmaven.test.failure.ignore=true
RUN mvn test -Dgroups=CREATE_TEST_PLAN
RUN mvn allure:aggregate
RUN mvn test-it:exportAutoTests

RUN rm -rf /app/certificates /app/src /app/target /app/test-result /app/.allure
RUN rm -f /app/Dockerfile /app/integration-tests.iml /app/pom.xml /app/readme.md /app/settings.xml /app/start_test.sh


Изменение имени системных переменных
URL->URLS_FILE_CONFIG
test_plan_name -> TEST_PLAN_NAME
test_groups->TEST_GROUPS

RUN mvn clean -Dsurefire.rerunFailingTestsCount=2 test -Dgroups=SmokeUI -Dmaven.test.failure.ignore=true
RUN mvn test -Dgroups=CREATE_TEST_PLAN
RUN mvn allure:serve
RUN mvn test-it:exportAutoTests
RUN mvn test-it:exportTestPlanResults -DtestPlanName=test_name_test_plan -DtestSuiteName=Регресс -DtestRunName=Регресс



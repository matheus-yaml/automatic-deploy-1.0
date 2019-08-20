**INSTALAR DOCKER:** <code>sudo curl –fsSL https://get.docker.com | bash</code> <br/>
**INSTALAR DOCKER-COMPOSE:** <code>sudo apt-get install docker-compose</code> <br/>

**Baixar imagens:** <br/>
<code>docker pull gitlab/gitlab-ce;</code>  <br/>
<code>docker pull jenkins/jenkins:lts;</code> <br/>
<code>docker pull python:3.7.4-alpine3.9;</code> <br/>
  
**Instanciar o gitlab:** <br/>
<code>docker-compose up -d gitlab</code> <br/>
  
**Instanciar o facebook:** <br/>
<code>docker-compose up -d facebook</code> <br/>
  
**Configurar o gitlab:** <br/> 
http://localhost/9090 <br/>
criar projeto facebook-flask <br/>
  
**Commitar os arquivos dentro de gitlab/app/facebook-flask para o GITLAB:** <br/>
<code>cd /home/$USER/automatic-deploy-1.0/gitlab/app/facebook-flask</code> <br/>
<code>git init</code> <br/>
<code>git remote add origin http://localhost:9090/root/facebook-flask</code> <br/>
<code>git add .</code> <br/>
<code>git commit -m 'redondinho'</code> <br/>
<code>git push origin master</code> <br/>
  
**Instanciar o jenkins:** <br/>
<code>docker-compose up -d jenkins</code> <br/>
  
**Configurar o jenkins:** <br/>
http://localhost:8080 <br/>
**Get token:** <br/>
<code>docker logs jenkins</code> <br/>
**Install suggested plugins** <br/>
pass <br/>
**Instalar seguintes plugins e reiniciar jenkins:** gitlab, slack notifications e publish over ssh  <br/>
http://localhost:8080/pluginManager/available <br/>

**Configurar server ssh** <br/>
http://localhost:8080/configure <br/>
Name: facebook <br/>
Hostname: 192.168.77.130 <br/>
Username: matheus <br/>
Remote Directory: /home/matheus/automatic-deploy-1.0/facebook <br/>
Advanced -> password <br/>
 
**Configurar job facebook no jenkins** <br/>

**Source code management** <br/>
Git[x] http://gitlab/root/facebook-flask <br/>
**Build triggers** <br/>
[x]Build when a change is pushed to GitLab. <br/>
-> avanced -> generete secret token <br/>
**Build** <br/>
ssh servers -> facebook
source files -> <code>**/*</code><br/>
Exec command: docker restart facebook <br/>
**Post-build actions** <br/>
[x] slack notifications <br/>
-> advanced <br/>
override url: https://automatic-deploy.slack.com/services/hooks/jenkins-ci/ <br/>
secret token: xxx <br/>

**Configurar REPOSITÓRIO no gitlab para enviar o hook para o jenkis sempre que houver algum evento (push)** <br/>
http://localhost:9090/root/facebook-flask/-/settings/integrations <br/>
URL: http://localhost:9090/root/facebook-flask/-/settings/integrations <br/>
Secret Token: Pegue no job do jenkins <br/>
 



# Launching-configuring-connecting-using-an-EC2-Instance

##

https://www.youtube.com/watch?v=74e1asRdR7k

##
app.js
const express = require('express');
const bodyParser = require('body-parser');

const {
  getTopics,
  getTopic,
  addNewTopic,
  addNewOpinion,
} = require('./data');

const app = express();

app.set('view engine', 'ejs');

app.use(bodyParser.urlencoded({ extended: false }));
app.use(express.static('public'));

app.get('/', function (req, res) {
  res.redirect('/topics');
});

app.get('/topics', async function (req, res) {
  const topics = await getTopics();
  res.render('index', { topics: topics });
});

app.get('/topics/:id', async function (req, res) {
  const topicId = req.params.id;
  const topic = await getTopic(topicId);
  res.render('topic', { topic: topic });
});

app.post('/topic', async function (req, res) {
  const topicData = req.body;
  await addNewTopic(topicData);
  res.redirect('/topics');
});

app.post('/share-opinion', async function (req, res) {
  const submittedData = req.body;
  const topicId = submittedData['topic-id'];
  const opinionData = {
    title: submittedData.title,
    user: submittedData.user,
    text: submittedData.text,
  };
  await addNewOpinion(topicId, opinionData);
  res.redirect(`/topics/${topicId}`);
});

app.listen(3000);
 
##
https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-18-140 ~]$


https://aws.amazon.com/amazon-linux-2/
https://docs.github.com/pt/enterprise-server@3.2/admin/installation/setting-up-a-github-enterprise-server-instance/installing-github-enterprise-server-on-aws
[ec2-user@ip-172-31-18-140 ~]$ sudo yum update 
[ec2-user@ip-172-31-18-140 ~]$ sudo yum install git 
[ec2-user@ip-172-31-18-140 ~]$
Is this ok [y/d/N]: y 
[ec2-user@ip-172-31-18-140 ~]$ git clone https://github.com/academind/aws-demos.git
[ec2-user@ip-172-31-18-140 ~]$ ls
[ec2-user@ip-172-31-18-140 ~]$ cd aws 
[ec2-user@ip-172-31-18-140 ~]$ cd aws-demos
[ec2-user@ip-172-31-18-140 aws-demos ]$ ls
[ec2-user@ip-172-31-18-140 aws-demos ]$ cd dynamic-website-basic/
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ ls
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ clear
https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ . ~/.nvm/nvm.sh
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ nvm install --lts
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ ls
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ npm install
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ node app.js
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ sudo mkdir -p /demo/data 
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ echo ‘{“topics” : []}’ | sude tee “/demo/data/data-storage.json” 
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ sudo groupadd www
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ sudo usermod -a -G www ec2-user
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ exit 
 
[ec2-user@ip-172-31-18-140 ~]$ chown -R root:www /demo/data 
[ec2-user@ip-172-31-18-140 ~]$ sudo  chown -R root:www /demo/data
[ec2-user@ip-172-31-18-140 ~]$ sudo chmod 2775 /demo/data 
[ec2-user@ip-172-31-18-140 ~]$ 
[ec2-user@ip-172-31-18-140 ~]$ find /demo/data -type d -exec sudo chmod 2775 {} + 
[ec2-user@ip-172-31-18-140 ~]$ find /demo/data -type f -exec sudo chmod 0664 {} + 
[ec2-user@ip-172-31-18-140 ~]$ ls
[ec2-user@ip-172-31-18-140 ~]$ cd aws-demos/
[ec2-user@ip-172-31-18-140 aws-demos ]$ ls
[ec2-user@ip-172-31-18-140 aws-demos ]$ cd dynamic-website-basic/ 
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ clear
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ node app.js
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ clear
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ sudo iptables -t nat -A PREROUTING -p tcp – dport 80 -j REDIRECT – to-ports 3000
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ node app.js 
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ 


https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-18-140 ~]$ cd aws-demos/dynamic-website-basic/
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ npm install pm2@latest -g
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ clear
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$ pm2 start app.js
[ec2-user@ip-172-31-18-140 dynamic-website-basic ]$

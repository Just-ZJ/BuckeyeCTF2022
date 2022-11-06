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
<img src="https://user-images.githubusercontent.com/54641137/200136116-053964d6-bf52-49a1-941b-4d346ad57c6e.png" width="50%"/>  

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








>### [crypto/powerball/easy]([https://scanbook.chall.pwnoh.io/](https://powerball.chall.pwnoh.io/))
<img src="https://user-images.githubusercontent.com/54641137/200136159-6a5e621a-9093-4168-a8a0-b610c3ee2fc6.png" width="50%"/>  

<details>
  <h3>Method: </h3>  
  
  Download `powerball.zip` and read through the `app.js`.
  Then, run `app.js` using the command `node app.js` a few times and log various variables. You would find the following facts: 
  <code></code>
  <ol>
    <li>The value of multiplier stays constant. <code>multiplier = 170141183460469231731687303715884105727n</code></li>
    <li>The value of <code>seed</code> used in the calculation of <code>nextRandomNumber()</code> is always the previous seed, <code>S<sub>n-1</sub></code>.</li>
    <li>The next seed, <code>S<sub>n</sub></code>, is the return value of <code>nextRandomNumber()</code></li>
    <li>The next seed, <code>S<sub>n</sub></code>, is what you need to get the winning ball for the flag.</li>
    <li>The value of modulus stays constant when generating all the flags in a single run of <code>app.js</code>.</li>
  </ol>

  ```
  function nextRandomNumber() {
    console.log("multiplier - " + multiplier);
    console.log("modulus - " + modulus);
    console.log("seed - " + seed);
    return (multiplier * seed) % modulus;
  }
  ```
 Therefore, since multiplier and seed is known to us, the only parameter we would need to find to figure out in `nextRandomNumber()` is the value of modulus. However, this is hard as reversing modulus operations is really really difficult (trust me on this. i've tried it.).
  
  A random idea that came up would be to get the Greatest Common Denominator of seed<sub>n</sub> & seed<sub>n-1</sub>, which works. This came from the inspiration of:

  >x % n = r, where x is the number, n is the divisor, r is the remainder.  
  Therefore, n * q + r = x, where q is the quotient.  
  Rewriting, x - r = n * q.  
  Following from our fact 5 above, n should stay the same for x<sub>1</sub> & x<sub>2</sub>. 
  Therefore, n should be a common factor of both x<sub>1</sub> & x<sub>2</sub>  
  
  >Checking my thought with examples:  
  
  >7 % n = 1         => 7 - 1 = 2 * 3  
  8 % n = 0         => 8 - 0 = 2 * 4  
  GCD(7 - 1, 8 - 1) = 2 and if n = 2, the above 2 statements are true.  
  
  >1515 % n = 3      => 1515 - 3 = 8 * 189  
  1531 % n = 3      => 1531 - 3 = 8 * 191  
  GCD(1515 - 3, 1531 - 3) = 8 and if n = 8, the above 2 statements are true.  
  
  Thus, to find the next seed, the formula would be (multiplier * prevSeed) % mod, where mod is the GCD(Seed<sub>1</sub> & Seed<sub>2</sub>). Then, i went on to testing if the GCD of Seed<sub>1</sub> & seed<sub>2</sub> would help get Seed<sub>3</sub> and it did.
  
  Solution: Get the first 3 flags from the website and replace seed0, seed1 and seed2 respectively in `decode()` to generate the wining ball.  
  Note: This works 100% only for the 4th seed generated. If the 4th is missed, refresh the webpage.
  ```
  function seedToBalls(n) {
    const balls = [];
    //10 balls
    for (let i = 0; i < 10; i++) {
      balls.push(Number(n % 100n));
      n = n / 100n;
    }
    console.log(balls);
    return balls;
  }
  
  const gcd = (a, b) => {
    if (!b) {
      return a;
    }
    return gcd(b, a % b);
  };

  const findNext = (multiplier, prevSeed, mod) => {
    return (multiplier * prevSeed) % mod;
  };

  const decode = () => {
    let seed0 = 101204381215958959726337149858943218784n;
    let seed1 = 238864599652411061782172846888836024917n;
    let seed2 = 53089622935316837292122757133819042852n;
    let multiplier = 170141183460469231731687303715884105727n;
    let mod = gcd(multiplier * seed0 - seed1, multiplier * seed1 - seed2);
    seedToBalls(findNext(multiplier, seed2, mod));
  };
  decode();
  ```
  
  Note: Process of solving can be found in `decode.js`
  
</details>

Flag: `buckeye{y3ah_m4yb3_u51nG_A_l1N34r_c0nGru3Nt1al_G3n3r4t0r_f0r_P0w3rB4lL_wA5nt_tH3_b3st_1d3A}`








>### [web/quizbot/beginner](https://discord.gg/q4qtnrKd)
<img src="https://user-images.githubusercontent.com/54641137/200197178-e00f605b-0528-4e9c-a419-ecb8dc10ff8a.png" width="50%"/>  

<details>
  
  ```
  async def finish_quiz(message):
    _, quiz_answers = message.content.split(" ", 1)
    quiz_answers = quiz_answers.split(":")
    if len(quiz_answers) != 7:
        await message.author.dm_channel.send("You didn't answer all the questions :/")
        return
    first, last, maiden, cc, pet, teacher, parent_meeting = quiz_answers
    await message.author.dm_channel.send(f"Thank you, {first}. We've analyzed your answers and assigned you the appropriate role!")
    role = discord.utils.get(guild.roles, name="Ask for username and password in Discord")
    member = guild.get_member(message.author.id)

    if member:
        await member.add_roles(role)
  ```
  
  The function that you would have to take note here would be `on_raw_reaction_add`. 
  ```
  @client.event
  async def on_raw_reaction_add(event):
      emoji = event.emoji
      user = client.get_user(event.user_id)
      member = guild.get_member(event.user_id)
      channel = client.get_channel(event.channel_id)
      message = await channel.fetch_message(event.message_id)

      if (
          message.author != client.user
          or user == client.user
      ):
          return

      lines = message.content.split("\n")[1:]         # message content would be splitted into an array by the delimiter '\n'
                                                      # lines would be an array starting at index 1. index 0 would be ignored
      for line in lines:
          try:
              line_reaction, role_name = line.strip().split(" ", 1)           # splits line by at most 1 space
                                                                              # line_reaction would be index 0
                                                                              # role_name would be index 1
          except ValueError:
              continue

          if str(emoji) == line_reaction:                                     # checks if the emoji of the reaction is the same as line_reaction
              role = discord.utils.get(guild.roles, name=role_name)           # looks for the role with role_name in the discord server
              if member:
                  await member.add_roles(role)                                # adds the role of role_name
  ```
  
  Therefore, our purpose is to add the `admin` role in the discord server using this chatbox, using a carefully crafted response that fits the above criteria and having that chatbox return that response through our first name.
  
  
  Therefore simply input the response below and add the 😂 reaction to the response from the bot
  ```
  !quiz asd
  :joy: admin
  :bimbo:brutus:440028476969420/222:wenis:sweaty:behind the taco bell`
  ```
  
  <img src="https://user-images.githubusercontent.com/54641137/200197935-c8a459d8-4003-4a53-8fd9-694bf9375b10.png" width="50%"/>
  
  Note: `!quiz asd\n:joy: admin\n:bimbo:brutus:440028476969420/222:wenis:sweaty:behind the taco bell` does not work! You would have to replace `\n` with a newline!
</details>

Flag: `buckeye{5tat3l355_m0r3_L1K3_DaT3L355}`






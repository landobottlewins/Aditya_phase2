# Cookies
> Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:64944/


## Solution:

- When searched for snickerdoodle the website changes to http://mercury.picoctf.net:64944/check but it doesnt change if something random like "aaa" is searched for.
- Since the challenge is called cookies I checked the cookies on firefox dev tools
- The cookie value is "0" when it's on snickerdoodle whereas it is "-1" for a random number
  <img width="1919" height="1016" alt="image" src="https://github.com/user-attachments/assets/59408767-9c4a-4a1e-83a5-6497bc9c8f6d" />

- So next I checked what happens if I put some other cookie... "pinwheel" has a value of 16
- This leads me to believe that the flag also must be a related to a cookie value.
- Upon manually entering values from 0 to 18, I found the flag on the 18. `picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}`




## Flag:

```
picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}
```

## Concepts learnt:

- cookies store data on client computer and are sent to server to display its associated information
- I can change the cookie value and see a different page


## Notes:

- I could have automated it using a script but my knowledge to is too elementary to write it by myself
- I discovered that burpsuite could be utilized for such purposes
- doing manually felt like i was doing it wrong... till i found the flag xD



***

<h2 id="wget">Wget</h2>
<p>There are other methods to download stuff on the internet. What makes wget good? Wow… you can do so much with such a tiny little command. The best aspect is <code>-r</code> or recursive retrieval which follows links from a website (to a certain depth). Mine all the things!</p>
<p>I think it also comes down to the same old idea of software and platform we embrace - it is all about preference and usage needs.</p>
<p>So let’s do a quick run of wget.</p>
<h3 id="windows-users">Windows Users</h3>
<p>Data Mining the Internet archive is hard on Windows. Unix is <code>luv</code>, Unix is <code>lyf</code>.</p>
<p>The tutorial was not at all geared for you. So beginner’s with the command line could be confused. First! Windows does not use the UNIX (read: also mac) command <code>sudo</code>. The command line or Powershell will not recognize this command.</p>
<p><code>sudo</code> executes a command as an administrator. You will have to type in your computer’s password. As a security measure, Unix will not show your password, even as dots as you write.</p>
<p>Do the following if <code>pip install</code> is not working</p>
<ol>
<li>Make sure python is at your root folder <code>C:\</code> So you will have <code>C:\Python27</code></li>
<li>In the command line at <code>C:\Python27</code> type <code>setx PATH "%PATH%;C:\Python27\Scripts"</code> (<em>NOTE:</em> You may need to restart your computer to run the next command)</li>
<li>Go back to the folder you want to work in - from that folder in the command line type <code>python -m pip install internetarchive</code></li>
<li>Profit</li>
</ol>

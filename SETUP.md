1. I fisrt went to the Oracle Cloud Free Tier website and set up a account so i would be allowed to create a instance for this webiste following steps in the youtube video https://www.youtube.com/watch?v=uyuHSFo0QQo (skip ubuntu part, use Oracle Linux 9)
2. After that i used the SSH keys that were givin and SSHed into my instance
   1) cd downloads/ssh-keys
   2) ssh -i .\ssh-key-2025-10-05.key opc@129.80.9.78
3. To update my instance I foloowed these in the instance terminal that were provided by a peer.
   1) sudo fallocate -l (that's an L) 8G /swapfile
   2) sudo chmod 600 /swapfile
   3) sudo mkswap /swapfile
   4) sudo swapon /swapfile
   5) vi /etc/fstab (to check if swapfile is there, below everything you can type in :wq (save/quit) or :q! (force quit), then press enter to exit it
   6) free -h or free -m to check to see if you have it working as well
   7) sudo systemctl daemon-reload
   8) sudo yum update -y
4. Getting that to work took ahile but it eventually all loaded and I was able to now move on and download node.js on my server instance by following the youtube video provided https://www.youtube.com/watch?v=AL7_oPEigpM
5. Using nano and touch i was able to copy and paste these templetes into my created Node.js app and index.html

Node.js Echo Server Template (index.js)
const express = require('express');
const path = require('path');
const app = express();
const PORT = process.env.PORT || 3000;

// Middleware to parse JSON and URL-encoded bodies
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Serve the homepage
app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
});

// The /echo endpoint for all request methods
app.all('/echo', (req, res) => {
    res.json({
        method: req.method,
        headers: req.headers,
        query: req.query,
        body: req.body
    });
});

// Start the server
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});

Server Home Page Template (index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Home Page</title>
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
</head>
<body class="bg-gray-100 flex items-center justify-center h-screen">
    <div class="text-center bg-white p-10 rounded-lg shadow-lg">
        <h1 class="text-4xl font-bold mb-4">Welcome to My Home Page</h1>
        <p class="text-gray-600">This page is under construction. More to come!</p>
    </div>
</body>
</html>
6. to get an auto start feature I used the terminal to type in 
 1) sudo npm install -g pm2
 2) pm2 start { your node app name}.js
 3) pm2 startup
 4) pm2 save
7. finally I used the last template to come into git hub and have it rederiect to my server.

GitHub Pages Redirect Template (index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 
        This meta tag redirects the user to your main web server after 0 seconds.
        Update the content URL to your server's IP address or domain name.
    -->
    <meta http-equiv="refresh" content="0; url=http://YOUR_SERVER_IP_OR_DOMAIN_HERE">
    <title>Redirecting...</title>
    <style>
        body { font-family: sans-serif; text-align: center; padding-top: 50px; }
    </style>
</head>
<body>
    <p>Redirecting...</p>
    <p>If you are not redirected, <a href="http://YOUR_SERVER_IP_OR_DOMAIN_HERE">click here</a>.</p>
</body>
</html>



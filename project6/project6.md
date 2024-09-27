**<u>DOCUMENTATION OF PROJECT 6</u>**

**<u>USING POSTMAN TO TEST APPLICATION</u>**

Open the VsCode

<img src="./media/image1.jpeg"
style="width:6.26806in;height:3.61389in" />

**USE BASH FOR THIS PROJECT**

Open a new terminal with bash

<img src="./media/image2.jpeg"
style="width:6.26806in;height:2.30764in" />

**USING GITBASH**

Create Project Environment Using the following command:

> *mkdir -p flask_api_project/{templates,static} && touch
> flask_api_project/app.py flask_api_project/templates/index.html
> flask_api_project/static/style.css && python -m venv
> flask_api_project/venv*

<img src="./media/image3.jpeg"
style="width:6.26806in;height:2.30764in" />

**DEVELOPING THE APPLICATION**

1.  In the app.py, paste the following code:

> *from flask import Flask, request, jsonify, render_template*
>
> *app = Flask(\_\_name\_\_)*
>
> *users = \[\]*
>
> *@app.route('/')*
>
> *def home():*
>
> *return render_template('index.html')*
>
> *@app.route('/users', methods=\['POST'\])*
>
> *def create_user():*
>
> *user = request.get_json()*
>
> *users.append(user)*
>
> *return jsonify(user), 201*
>
> *@app.route('/users', methods=\['GET'\])*
>
> *def get_users():*
>
> *return jsonify(users), 200*
>
> *@app.route('/users/\<int:user_id\>', methods=\['GET'\])*
>
> *def get_user(user_id):*
>
> *user = next((u for u in users if u\['id'\] == user_id), None)*
>
> *return jsonify(user), 200 if user else 404*
>
> *@app.route('/users/\<int:user_id\>', methods=\['PUT'\])*
>
> *def update_user(user_id):*
>
> *user = request.get_json()*
>
> *index = next((i for i, u in enumerate(users) if u\['id'\] ==
> user_id), None)*
>
> *if index is not None:*
>
> *users\[index\] = user*
>
> *return jsonify(user), 200*
>
> *return '', 404*
>
> *@app.route('/users/\<int:user_id\>', methods=\['DELETE'\])*
>
> *def delete_user(user_id):*
>
> *global users*
>
> *users = \[u for u in users if u\['id'\] != user_id\]*
>
> *return '', 204*
>
> *if \_\_name\_\_ == '\_\_main\_\_':*
>
> *app.run(debug=True)*

<img src="./media/image4.jpeg"
style="width:6.26806in;height:2.65417in" />

2.  In the **index.html** in the templates directory paste the code
    below:

> \<!DOCTYPE html\>
>
> \<html lang="en"\>
>
> \<head\>
>
> \<meta charset="UTF-8"\>
>
> \<meta name="viewport" content="width=device-width,
> initial-scale=1.0"\>
>
> \<title\>API-Based Application\</title\>
>
> \<link rel="stylesheet" href="{{ url_for('static',
> filename='style.css') }}"\>
>
> \</head\>
>
> \<body\>
>
> \<h1\>User Management\</h1\>
>
> \<form id="userForm"\>
>
> \<input type="text" id="name" placeholder="Name" required\>
>
> \<input type="email" id="email" placeholder="Email" required\>
>
> \<button type="submit"\>Add User\</button\>
>
> \</form\>
>
> \<ul id="userList"\>\</ul\>
>
> \<script\>
>
> document.getElementById('userForm').addEventListener('submit', async
> function (event) {
>
> event.preventDefault();
>
> const name = document.getElementById('name').value;
>
> const email = document.getElementById('email').value;
>
> const response = await fetch('/users', {
>
> method: 'POST',
>
> headers: {
>
> 'Content-Type': 'application/json'
>
> },
>
> body: JSON.stringify({ name, email })
>
> });
>
> const user = await response.json();
>
> document.getElementById('userList').innerHTML += \`\<li\>\${user.name}
> (\${user.email})\</li\>\`;
>
> });
>
> \</script\>
>
> \</body\>
>
> \</html\>

<img src="./media/image5.jpeg"
style="width:5.04255in;height:2.13523in" />

3.  In the **style.css** in the static directory paste this code below
    there:

> body {
>
> font-family: Arial, sans-serif;
>
> margin: 20px;
>
> }
>
> form {
>
> margin-bottom: 20px;
>
> }
>
> input {
>
> margin-right: 10px;
>
> }

<img src="./media/image6.jpeg"
style="width:6.26806in;height:2.65417in" />

4.  Running the Application

<img src="./media/image7.jpeg"
style="width:4.31915in;height:1.82892in" />

5.  Open your browser and go to http://127.0.0.1:5000 to see your
    application.

<img src="./media/image8.jpeg"
style="width:5.65957in;height:1.11235in" />

<img src="./media/image9.jpeg"
style="width:5.65903in;height:1.11224in" />

**TESTING THE API USING POSTMAN**

1.  Install Postman

<img src="./media/image10.jpeg"
style="width:6.26806in;height:1.04444in" />

2.  Create a new request

<img src="./media/image11.jpeg"
style="width:4.79787in;height:1.77914in" />

3.  Set the request type to POST and enter <http://127.0.0.1:5000/users>

<img src="./media/image12.jpeg"
style="width:6.26806in;height:2.32431in" />

4.  Go to the Body tab, select raw, and choose JSON from the dropdown.

<img src="./media/image13.jpeg"
style="width:6.26806in;height:2.32431in" />

5.  Enter the following JSON data and Click Send and check the response.

> {
>
> "id": 1,
>
> "name": "your name",
>
> "email": "yourname@example.com"
>
> }

<img src="./media/image14.jpeg"
style="width:6.26806in;height:3.10764in" />

The result show that our application is working.

**WHAT I PERSONALLY LEARNT**

For the Postman to work fine with an application running locally on my
machine, I have to run a Postman Agent. This will make you avoid an
error like the one below:

<img src="./media/image15.jpeg"
style="width:6.26806in;height:3.10903in" />

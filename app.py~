#!/usr/bin/env python
"""The MIT License (MIT)

Copyright (c) 2013 Seth Nash

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE."""
from flask import render_template, Flask, request
import json
import oauth2 as oauth
import keyring

app=Flask(__name__)
app.config.from_pyfile("config.py")
consumer=oauth.Consumer(keyring.get_password(app.config["KEYRING_NAME"], "CONSUMER_KEY"), keyring.get_password(app.config["KEYRING_NAME"], "CONSUMER_SECRET"))#Set with keyring in terminal for now
token=oauth.Token(keyring.get_password(app.config["KEYRING_NAME"], "ACCESS_KEY"), keyring.get_password(app.config["KEYRING_NAME"], "ACCESS_SECRET"))#Rewrite so it can work for multiple services.
client=oauth.Client(consumer, token)

@app.route("/updates")
@app.route("/updates.py")
def updates_page():
	return render_template("updates.html", updates=users.updates)

		

@app.route("/retrieve.py")
@app.route("/retrieve.json")
@app.route("/retrieve_updates.json")
@app.route("/retrieve_updates.json")#I have no idea why I do this.
def retrieve_updates():
	if request.args.get("service")=="twitter" or request.args.get("service")==None:
		r, d=client.request("https://api.twitter.com/1.1/statuses/user_timeline.json?screen_name=%s&count=%s" %(app.config["ACCOUNTS"]["twitter"], app.config["STATUS_COUNT"]))
		tweets=[]
		for tweet in json.loads(d):
			tweets.append({"text":tweet["text"], "created_at":tweet["created_at"], "retweeted":tweet["retweeted"], "user":tweet["user"]})
		return json.dumps(tweets)

@app.route("/admin")
@app.route("/admin.py")
def admin_page():
	return render_template("admin.html")

if __name__=="__main__":
	app.run(debug=True)

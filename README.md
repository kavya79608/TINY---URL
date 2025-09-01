from flask import Flask, request, redirect, render_template
import random
import string

app = Flask(_name_)

url_map = {}

def generate_short_url():
    return ''.join(random.choices(string.ascii_letters + string.digits, k=5))

@app.route('/', methods=['GET', 'POST'])
def home():
    if request.method == 'POST':
        long_url = request.form['long_url']
        short_url = generate_short_url()
        url_map[short_url] = long_url
        return f'Short URL: <a href="/{short_url}">http://localhost:5000/{short_url}</a>'
    return '''
        <form method="post">
            Long URL: <input name="long_url" required>
            <input type="submit">
        </form>
    '''

@app.route('/<short_url>')
def redirect_url(short_url):
    long_url = url_map.get(short_url)
    if long_url:
        return redirect(long_url)
    return "URL not found!"

if _name_ == '_main_':
    app.run(debug=True)

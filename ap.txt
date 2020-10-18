from flask import Flask,request, url_for, redirect, render_template
import pickle
import numpy as np
import pyttsx3
import webbrowser

x= pyttsx3.speak("Welcome to Snooz...")
webbrowser.open("http://127.0.0.1:5000/")
app = Flask(__name__)

model=pickle.load(open('mode2.pkl','rb'))


@app.route('/')
def hello_world():
    pyttsx3.speak("Let's get started")
    return render_template("index.html")



database={'akshay':'aksh','abhishek':'abhi','shreya':'shrek' , 'aarushi':'aalu' , 'guest':'123'}
@app.route('/Quick_sleep_checkup',methods=['POST','GET'])
def login():
    name1=request.form['username']
    pwd=request.form['password']
    if name1 not in database:
	    return render_template('index.html',info='Invalid User')
    else:
        if database[name1]!=pwd:
            return render_template('index.html',info='Invalid Password')
        else:
	         return render_template('predict.html',name=name1)


@app.route('/predict',methods=['POST','GET'])
def predict():
    int_features=[int(x) for x in request.form.values()]
    final=[np.array(int_features)]

    prediction=model.predict(final)

    return render_template('predict.html',output =prediction )

    if prediction>6.0:
        return render_template('predict.html',output='You have healthy sleep schedule')
    else:
        return render_template('predict.html',output='Your need to work hard')

@app.route('/Lets_check_your_calorie', methods=['POST','GET'])
def calorie():
        return render_template('calo.html')


if __name__ == '__main__':
    app.run(debug=True)

#
# "

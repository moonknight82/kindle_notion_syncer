FROM python:3.8

# install google chrome
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
RUN apt-get -y update
RUN apt-get install -y google-chrome-stable

# install chromedriver
RUN apt-get install -yqq unzip
RUN wget -O /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip chromedriver -d /usr/local/bin/

# install selenium
RUN pip install selenium

# create folder app
RUN mkdir /app
# copy test file
COPY test_webscrapping.py /app

# go to your directory of project remove # the following line to copy your projecto to the container.
#COPY . /app

#if you need to use some differents libs , use 
#RUN pip install -r requirements.txt
#or if you dont have requirements.txt file user the following example (try to use always requirements.txt)
#pip install numpy

# change yourscriptname for the name of your python file.
#CMD python /app/yourscriptname.py

# set display port to avoid crash
ENV DISPLAY=:99

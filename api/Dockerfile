FROM python:3.7.7

WORKDIR /app

# Install Java 11 for Pyserini	
RUN echo 'deb http://ftp.debian.org/debian stretch-backports main' | tee /etc/apt/sources.list.d/stretch-backports.list	
RUN apt-get update	
RUN apt-get install openjdk-11-jdk -y	

# Install Python dependencies
COPY requirements.txt /app/
RUN pip install -r requirements.txt

COPY . /app/

CMD ["uvicorn", "app.main:app", "--reload"]

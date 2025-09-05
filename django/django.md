# installation
run powershell as adminstrator --> win + x
## virtual environment
`project-path>> pip install virtualenv`
`>>python -m venv C:\path\to\new\virtual\environment` [see page](https://docs.python.org/3/library/venv.html#creating-virtual-environments) [see title](https://docs.python.org/3/library/venv.html#creating-virtual-environments:~:text=Creating%20virtual%20environments%C2%B6)
`>> cd script`
`script>> activate`
## django
[see topic](https://docs.djangoproject.com/en/5.2/topics/install/#install-the-django-code:~:text=Install%20the%20Django%20code%C2%B6)
`script>> pip freeze` نشان میدهد که چه پکیج هایی را نصب کرده است
`script>> cd..`
# start an app
[see topic](https://docs.djangoproject.com/en/5.2/intro/tutorial01/#:~:text=Documentation%20version%3A%205.2-,Writing%20your%20first%20Django%20app%2C%20part%201,-%C2%B6)
`appFolder>> django-admin startproject <projectname> .`
نقطه در انتها باعث میشود که درون فودر دوباره فولدری به نام همان اپ ایجاد نشود
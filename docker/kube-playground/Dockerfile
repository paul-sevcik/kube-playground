FROM python:3.6-onbuild
EXPOSE 5000
RUN python /usr/src/app/setup.py install
RUN FLASK_APP=kube_playground python /usr/src/app/kube_playground/kube_playground.py initdb
ENV FLASK_APP=kube_playground
ENV PYTHONUNBUFFERED=1
CMD python /usr/src/app/kube_playground/kube_playground.py run --host=0.0.0.0

ARG DOCKER_TAG=latest
ARG VERSION_BASE=v2.32.4
FROM graphistry/graphistry-forge-base:${VERSION_BASE}

EXPOSE 8501

# making directory of app
WORKDIR /app

COPY python/requirements-app.txt python/requirements-system.txt ./
RUN source activate rapids && pip install -r requirements-system.txt
RUN source activate rapids && pip install -r requirements-app.txt

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
RUN mkdir -p /root/.streamlit \
&& bash -c 'echo -e "\
[general]\n\
email = \"\"\n\
" > /root/.streamlit/credentials.toml' \
&& bash -c 'echo -e "\
[server]\n\
enableXsrfProtection = false\n\ 
enableCORS = false\n\
[browser]\n\
gatherUsageStats = false\n\
" > /root/.streamlit/config.toml'

#Note no trailing slash
ENV BASE_URL=http://localhost:8501/dashboard
ENV BASE_PATH=dashboard/
ENV LOG_LEVEL=ERROR

COPY docker/entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/apps/entrypoint.py"]

#Assume volume mount src/python as /apps/ (hot module reloading)
ARG image="openjdk"
ARG tag="11-jdk-slim"

FROM ${image}:${tag}

ARG Maitener=""

# On retire les priviléges de l'utilisateur root.

RUN useradd user

RUN usermod -a -G user user

USER user:user

# On démarre l'application Spring à l'aide de java.

ENTRYPOINT ["java","-jar","app.jar"]

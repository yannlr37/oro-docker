FROM rabbitmq:3.9-management

RUN echo "rabbitmq 3.19 management"

COPY ./rabbitmq_delayed_message_exchange-3.9.0.ez /opt/rabbitmq/plugins/
RUN rabbitmq-plugins enable rabbitmq_delayed_message_exchange
FROM nginx:1.28.2-alpine3.23-slim

RUN chown -R nginx:nginx /var/cache/nginx /var/run /var/log/nginx /run

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 4011

USER nginx

CMD ["nginx", "-g", "daemon off;"]
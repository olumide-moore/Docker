FROM nginx:1.27.0

# Remove the default nginx html directory
RUN rm -rf /usr/share/nginx/html

# Copy the static files to the nginx directory
COPY static /usr/share/nginx/html 

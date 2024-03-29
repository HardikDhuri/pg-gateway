# Use Alpine Linux
FROM alpine:latest

# Add non-root user for PgBouncer
RUN adduser -D pgbouncer

# Install PgBouncer and dependencies
RUN apk add --no-cache pgbouncer

# Create directory for pidfile
RUN mkdir -p /var/run/pgbouncer && chown -R pgbouncer:pgbouncer /var/run/pgbouncer

# Copy PgBouncer configuration files into the container
COPY pgbouncer.ini /etc/pgbouncer/pgbouncer.ini
COPY userlist.txt /etc/pgbouncer/userlist.txt

# Change ownership of configuration files
RUN chown pgbouncer:pgbouncer /etc/pgbouncer/pgbouncer.ini /etc/pgbouncer/userlist.txt

# Expose the port PgBouncer listens on
EXPOSE 6432

# Switch to the non-root user
USER pgbouncer

# Run PgBouncer
CMD ["pgbouncer", "/etc/pgbouncer/pgbouncer.ini"]
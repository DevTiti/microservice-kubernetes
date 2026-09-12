# --- Build stage: install gems with dev headers available ---
FROM ruby:3.3-slim AS build

WORKDIR /app

# Native build tools + Postgres client headers needed to compile the `pg` gem
RUN apt-get update -qq && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

COPY Gemfile Gemfile.lock* ./
RUN bundle config set --local without 'development test' && \
    bundle install --jobs 4 --retry 3

COPY . .

# --- Runtime stage: slim image, only the runtime libpq needed ---
FROM ruby:3.3-slim

WORKDIR /app

RUN apt-get update -qq && apt-get install -y --no-install-recommends \
    libpq5 \
    && rm -rf /var/lib/apt/lists/*

# Run as a non-root user
RUN useradd --create-home --shell /bin/bash appuser

COPY --from=build /usr/local/bundle /usr/local/bundle
COPY --from=build /app /app

RUN chown -R appuser:appuser /app
USER appuser

ENV NOTIFICATION_DB_URL="postgres://microapp:microapp123@127.0.0.1:5432/notification_db"

EXPOSE 8086

# Adjust filename below if your entrypoint isn't app.rb
CMD ["ruby", "app.rb"]
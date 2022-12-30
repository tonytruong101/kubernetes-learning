# Stage 1: Build the application
FROM ruby:2.7 as builder

# Install dependencies and build the application
RUN apt-get update && apt-get install -y \
  build-essential \
  nodejs \
&& rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the Gemfile and Gemfile.lock
COPY Gemfile* ./

# Install the gems
RUN bundle install --jobs 4 --retry 3

# Copy the rest of the application code
COPY . .

# Precompile the assets
RUN bundle exec rails assets:precompile

# Stage 2: Build the final image
FROM ruby:2.7

# Install the necessary dependencies
RUN apt-get update && apt-get install -y \
  libpq-dev \
  nodejs \
&& rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the compiled assets and gems from the builder stage
COPY --from=builder /app/public/assets ./public/assets
COPY --from=builder /app/vendor/bundle ./vendor/bundle

# Copy the rest of the application code
COPY . .

# Set the Rails environment
ENV RAILS_ENV production

# Expose the default Rails port
EXPOSE 3000

# Run the Rails server
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]


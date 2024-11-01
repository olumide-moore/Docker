#Seperate the build and run stages to reduce the size of the final image
#The builder stage will produce the wheels for the project and its dependencies
FROM python:3.12-slim AS builder

# Set the working directory
WORKDIR /app

#Copy the pyproject.toml and requirements.txt files (pyproject.toml is for poetry and requirements.txt is for pip)
COPY pyproject.toml requirements.txt ./
# Generate .whl file for each packages in requirements.txt file into the /wheels directory, ignore their dependencies
RUN pip wheel --no-cache-dir --no-deps --wheel-dir wheels -r requirements.txt
# Copy the source code to the working directory
COPY src src
# Generate a .whl file for the project/source code also into the /wheels directory
RUN pip wheel --no-cache-dir --no-deps --wheel-dir wheels .

# Start a new stage (this is the final stage and will only install and run the pre-built wheels)
FROM python:3.12-slim AS runner
#Copy the wheels from the previous stage
COPY --from=builder /app/wheels /wheels
# Install the wheels from the /wheels directory and remove the directory
RUN pip install --no-cache /wheels/* && rm -rf /wheels
#The expose does not do anything, it is just for documentation purposes
EXPOSE 8000

# Default command to run the app if no command is provided
CMD ["uvicorn","mysite.main:app","--host","0.0.0.0", "--port", "8000"]
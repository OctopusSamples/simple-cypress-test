This is a simple Cypress test configured to create a HTML report.

This test can be run in Octopus from a worker container built with the following `Dockerfile`:

```
FROM cypress/included:9.7.0
RUN apt-get update; \
    apt-get install -y libicu67
RUN npm install -g inline-assets mochawesome
ENTRYPOINT []
```

The script to run this test (assuming the test itself is saved under `cypresstest`) is:

```
cd cypresstest
cypress run > output.txt
RESULT=$?
inline-assets mochawesome.html selfcontained.html
new_octopusartifact "${PWD}/selfcontained.html" "selfcontained.html"
exit ${RESULT}
```

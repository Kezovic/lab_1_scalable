# Air Quality Dashboard
![Hopsworks Logo](../titanic/assets/img/logo.png)

{% include air-quality.html %}

{% assign street = "tokyo,oslo,london" | split: "," %}

{% for city in cities %}
{{ city | capitalize }} Air Quality
![Forecast](./assets/img/pm25forecast{{ street }}.png)

# Model Performance Monitoring
1-Day Hindcast: Predictions vs Outcomes

![Hindcast](./assets/img/pm25_hindcast1day{{ street }}.png)

{% endfor %}

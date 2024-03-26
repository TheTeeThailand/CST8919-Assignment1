# Assignment 1: Monitoring Solution for Kubernetes Application with Gmail SSO
# Group Member

- Parvezbhai Malek
- Yash Solanki
- Thep Rungpholsatit

Introduction

	This assignment seeks to create a robust monitoring solution for an application deployed on Kubernetes, integrating Single Sign-On (SSO) via Gmail for user authentication. The system retrieves application events through a syslog server and forwards the data to Azure Log Analytics for analysis. Grafana is then employed to visualize telemetry data from Azure Log Analytics, offering valuable insights into the application's performance and usage trends.

Part 1 Application Setup in Kubernetes

1.	Deployment of Web-APP Google SSO Pod

Firstly, we created a simple web application using python:

app.py:

This code is a Flask application that implements OAuth2 authentication with Google using the requests_oauthlib library. Let's break down the key components:

Flask Setup:

The Flask application is created and initialized.
A secret key is generated for session management.

OAuth Configuration:

•	CLIENT_ID, CLIENT_SECRET, and REDIRECT_URI are provided by Google when you register your application with the Google API Console.
•	SCOPE specifies the permissions that the application requests from the user.
•	AUTHORIZATION_BASE_URL is the URL where the user is redirected to authorize the application.
•	TOKEN_URL is the URL used to exchange the authorization code for an access token.

Routes:

/: Redirects to the login route.
/login: Initiates the OAuth2 authentication process by redirecting the user to Google's authorization URL.
/login/callback: Handles the callback from Google after the user authorizes the application.
OAuth Flow:
•	When a user visits /login, they are redirected to Google's authorization URL, where they can log in and grant permissions to the application.
•	After authorization, the user is redirected back to /login/callback.
•	In the callback route, the code checks if the state parameter matches the one stored in the session to prevent CSRF attacks.
•	It then creates an OAuth2Session instance with the client ID, redirect URI, and state retrieved from the session.
•	Using the authorization response received in the callback, it exchanges the authorization code for an access token.
•	With the access token, it fetches the user's information from Google's user info endpoint.
Finally, it returns a message indicating successful authentication along with the user's information.

Main Execution:

•	If the script is executed directly, the Flask application runs in debug mode with SSL enabled using an ad-hoc SSL context.
•	This code provides a basic foundation for implementing OAuth2 authentication with Google in a Flask application. It handles the authentication flow and retrieves user information after successful authorization.

![Screen Shot 2567-03-25 at 21 07 26](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/da26b0c6-9304-494b-b7af-d80865afb2db)

Login.html:

his HTML code provides a simple interface for users to initiate the login process using their Google account. Upon clicking the "Login with Google" link, the user is redirected to the appropriate route in the web application to handle the authentication flow.

![Screen Shot 2567-03-25 at 21 12 08](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/32218b88-9ce9-49f7-a9d1-d653cb7157d0)

•	We use OAuth for Authentication:

![Screen Shot 2567-03-25 at 21 13 08](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/c7c48d8c-055d-4eb0-90eb-440ebf3679e7)

Output:

![Screen Shot 2567-03-25 at 21 13 46](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/9c41569d-8ceb-4b45-bda0-b2c4ea584379)

Authentication Successful:
Localhost: https://127.0.0.1:5000

![Screen Shot 2567-03-25 at 21 14 25](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/b44217a6-3f80-464d-9098-02896ec4536a)

Build Dockerfile:

![Screen Shot 2567-03-25 at 21 15 06](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/ce327b79-e695-4c9b-b39c-a80bfcf1db66)


1.	Deploying application on Kubernetes:

•	Within its architecture, the Kubernetes cluster hosts a variety of applications and services, acting as the fundamental infrastructure. It is made up of several pods that can communicate with one another to provide an integrated environment for the hosted applications. Kubernetes Services, which guarantee smooth connectivity between the pods and with outside networks, enable networking inside this cluster and allow it to reach outside its internal boundaries. 
•	Azure Monitoring is used for cluster monitoring and performance analysis. This tool, which offers insights into the cluster's performance and health data, is essential to guaranteeing its proper operation. 

Deployment.yaml:

![Screen Shot 2567-03-25 at 21 16 04](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/cf4f30df-b1e5-443d-9c63-236235219d21)

Kubectl get svc:

![Screen Shot 2567-03-25 at 21 16 24](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/73c0ce55-dddf-41b3-84ae-b1aa12207306)

 Part 2 Syslog Server Configuration

- Install rsyslog on ubuntu

![Screen Shot 2567-03-25 at 21 16 57](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/aabba631-1dc4-4af3-99f1-c9c251415a6a)

![Screen Shot 2567-03-25 at 21 17 11](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/24b43469-7001-4c89-9e04-b4b72f53f904)

![Screen Shot 2567-03-25 at 21 17 25](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/b1ce5260-38ed-49be-b3d2-8e068821e260)

- Configure Rsyslog Server

![Screen Shot 2567-03-25 at 21 17 52](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/6e6b266e-e5d1-47b9-9935-b660b5aeaf0c)

- Test Logging

![Screen Shot 2567-03-25 at 21 18 26](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/f3b67ee9-6359-4e8c-953b-d22a7309c7e4)

Part 3 Integration with Azure Log Analytics

![Screen Shot 2567-03-25 at 21 18 54](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/e0c05623-d908-4fcf-a493-476616dd117a)

![Screen Shot 2567-03-25 at 21 19 16](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/ae9c9da9-bb01-440d-b7a2-dd2d14560828)

![Screen Shot 2567-03-25 at 21 19 36](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/2069bf1b-257a-4a71-86fc-fcd29062d003)

![Screen Shot 2567-03-25 at 21 19 52](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/97a072c6-82fc-45cb-9525-acecf398d24d)

![Screen Shot 2567-03-25 at 21 20 06](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/14c2c96a-1889-4f53-8263-13734ed4575d)

Part 4 Data Visualization with Grafana

![Screen Shot 2567-03-25 at 21 20 28](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/41d9c0b1-cfd5-45f8-ba3d-c2681fe2cf62)

![Screen Shot 2567-03-25 at 21 20 42](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/e6277105-7741-4683-8613-1d92b69dbc74)

![Screen Shot 2567-03-25 at 21 20 56](https://github.com/TheTeeThailand/CST8919-Assignment1/assets/144491675/adefbf37-fc23-4be1-8890-e0bd6ce05352)












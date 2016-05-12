# Natural Language Classifier Demo
  The IBM Watson&trade; Natural Language Classifier service applies deep learning techniques to make predictions about the best predefined classes for short sentences or phrases. The classes can trigger a corresponding action in an application, such as directing a request to a location or person, or answering a question. After training, the service returns information for texts that it hasn't seen before. The response includes the name of the top classes and confidence values.

This lab will walk a user through the creation and testing of a natural language classifier. The sample data set includes a intents around health care information. The project is based on the base [NLC demo against weather](https://github.com/watson-developer-cloud/natural-language-classifier-nodejs).


## Create A Classifier

1. You need a Bluemix account. If you don't have one, [sign up][sign_up].

1. Download and install the [Cloud-foundry CLI][cloud_foundry] tool if you haven't already.

1. Provision an NLC service by going through the Bluemix catalog. Or using the CF command line tools.

	1. [CF Option] Connect to Bluemix with the command line tool.

  	```sh
  	$ cf api https://api.ng.bluemix.net
  	$ cf login -u <your user ID>
  	```

  1. [CF Option] Create the Natural Language Classifier service.

  	```sh
  	$ cf create-service natural_language_classifier standard natural-language-classifier-standard
  	```
1. [Create and train](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/nl-classifier/get_start.shtml#create) a classifier model using the NIDDK training data (in training/niddk_nlc_train.csv).

  1. Gather the credentials for the provisioned NLC service.
  1. Train the classifier using the following curl command

  	```sh
  	curl -u "{credentials_username}":"{credentials_password}" -F training_data=@niddk_nlc_train.csv -F training_metadata="{\"language\":\"en\",\"name\":\"Test Classifier\"}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers"
  	```
  1. Take note of the `<classifier-id>`.
  1. Wait until the classifier becomes available. Use the following command to check status:
  	```sh
  	curl -u "{credentials_username}":"{credentials_password}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers/{classifier_id}"
  	```
1. (Optional) Test the classifier using the following command:
    	```sh
    	curl -G -u "{credentials_username}":"{credentials_password}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers/{classifier_id}/classify?test=What%20are%20the%20symptoms%20of%20diabetes"
    	```

## Build and Run Test Application Locally
1. Download and install [Node.js](http://nodejs.org/) and [npm](https://www.npmjs.com/).

1. Configure the code to connect to your service:
  1. Copy the credentials from your natural language classifier service in Bluemix. Run the following command:

      		```sh
      		$ cf env <application-name>
      		```

      		Example output:

      		```sh
      		System-Provided:
      		{
      		  "VCAP_SERVICES": {
      			"natural_language_classifier": [
      			  {
      				"credentials": {
      				  "password": "<password>",
      				  "url": "<url>",
      				  "username": "<username>"
      				}
      				"label": "natural-language-classifier",
      				"name": "natural-language-classifier-standard",
      				"plan": "standard",
      				"tags": [
      				  ...
      				]
      			  }
      			]
      		  }
      		}
      		```
  1. Copy `username`, `password`, and `url` from the credentials.

  1. Open the `app.js` file and paste the username, password, and url credentials for the service.

  1. In the `app.js` file paste the "Classifier ID". Save the `app.js` file.

1. Install the Natural Language Classifier Node.js package:
	1. Change to the new directory that contains the project.
	2. Run the following command:node

      	```node
      	$ npm install
      	```
1. Run the following command to start the application:

      	```node
      	node server.js
      	```

1. Point your browser to [http://localhost:3000](http://localhost:3000) and test the service.

## Extending The Classifier
1. Extend the classifier ground truth with a new class label.
  1. Open the NIDDK training data (training/niddk_nlc_train.csv).
  1. Add training elements for a new label (for example "drug_dosage").
  1. Add 5 or more training samples for the new label.
  1. Save the new file as niddk_nlc_train_v2.csv

1. Train a new classification model.
  1. Gather the credentials for the provisioned NLC service.
  1. Train the classifier using the following curl command

  	```sh
  	curl -u "{credentials_username}":"{credentials_password}" -F training_data=@niddk_nlc_train_v2.csv -F training_metadata="{\"language\":\"en\",\"name\":\"Test Classifier V2\"}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers"
  	```
  1. Take note of the `<classifier-id>`.
  1. Wait until the classifier becomes available. Use the following command to check status:
  	```sh
  	curl -u "{credentials_username}":"{credentials_password}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers/{classifier_id}"
  	```
1. (Optional) Test the classifier using the following command:
    	```sh
    	curl -G -u "{credentials_username}":"{credentials_password}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers/{classifier_id}/classify?test={Encoded question}"
    	```
1. Modify the `app.js` file by pasting the new "Classifier ID". Save the `app.js` file and test using the sample application


## Troubleshooting

* For more details about the service, see the [documentation][nlc_docs] for the Natural Language Classifier.

## License

  This sample code is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE).  
  This sample uses [jquery](https://jquery.com/) which is MIT license
## Contributing

  See [CONTRIBUTING](CONTRIBUTING.md).

## Open Source @ IBM
  Find more open source projects on the [IBM Github Page](http://ibm.github.io/)


[deploy_track_url]: https://github.com/cloudant-labs/deployment-tracker
[cloud_foundry]: https://github.com/cloudfoundry/cli
[getting_started]: http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/getting_started/
[nlc_docs]: http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/nl-classifier.html
[sign_up]: https://console.ng.bluemix.net/registration/

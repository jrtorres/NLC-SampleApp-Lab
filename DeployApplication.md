# Natural Language Classifier Demo

## Getting started

1. You need a Bluemix account. If you don't have one, [sign up][sign_up]. Experimental Watson Services are free to use.

1. Download and install the [Cloud-foundry CLI][cloud_foundry] tool if you haven't already.

1. Edit the `manifest.yml` file and change `<application-name>` to something unique. The name you use determines the URL of your application. For example, `<application-name>.mybluemix.net`.

	```
	applications:
	- services:
	  - natural-language-classifier-standard
	  name: <application-name>
	  command: node app.js
	  path: .
	  memory: 128M
	```

1. Connect to Bluemix with the command line tool.

	```sh
	$ cf api https://api.ng.bluemix.net
	$ cf login -u <your user ID>
	```

1. Create the Natural Language Classifier service in Bluemix.

	```sh
	$ cf create-service natural_language_classifier standard natural-language-classifier-standard
	```

1. Push your app to make it live:

	```sh
	$ cf push
	```

1. [Create and train](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/nl-classifier/get_start.shtml#create) the NLC service using the weather training data. Take note of the `<classifier-id>`.

1. To configure the [app.js](app.js#L48) file to use your classifier, export the classifier ID as an environment variable.

	```sh
	$ cf set-env <application-name> CLASSIFIER_ID <classifier-id>
	```

1. Finally, restage the application to ensure the environment variable is set.

	```sh
	$ cf restage <application-name>
	```

	For more details about developing applications that use Watson Developer Cloud services in Bluemix, see [Getting started with Watson Developer Cloud and Bluemix][getting_started].

## Troubleshooting

* The main source of troubleshooting and recovery information is the Bluemix log. To view the log, run the following command:

  ```sh
  $ cf logs <application-name> --recent
  ```

* For more details about the service, see the [documentation][nlc_docs] for the Natural Language Classifier.

## License

  This sample code is licensed under Apache 2.0. Full license text is available in [LICENSE](LICENSE).  
  This sample uses [jquery](https://jquery.com/) which is MIT license
## Contributing

  See [CONTRIBUTING](CONTRIBUTING.md).

## Open Source @ IBM
  Find more open source projects on the [IBM Github Page](http://ibm.github.io/)

### Privacy Notice

This node sample web application includes code to track deployments to Bluemix and other Cloud Foundry platforms. The following information is sent to a [Deployment Tracker][deploy_track_url] service on each deployment:

* Application Name (`application_name`)
* Space ID (`space_id`)
* Application Version (`application_version`)
* Application URIs (`application_uris`)

This data is collected from the `VCAP_APPLICATION` environment variable in IBM Bluemix and other Cloud Foundry platforms. This data is used by IBM to track metrics around deployments of sample applications to IBM Bluemix. Only deployments of sample applications that include code to ping the Deployment Tracker service will be tracked.

### Disabling Deployment Tracking

Deployment tracking can be disabled by removing `require('cf-deployment-tracker-client').track();` from the beginning of the `server.js` file at the root of this repo.

[deploy_track_url]: https://github.com/cloudant-labs/deployment-tracker
[cloud_foundry]: https://github.com/cloudfoundry/cli
[getting_started]: http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/getting_started/
[nlc_docs]: http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/nl-classifier.html
[sign_up]: https://console.ng.bluemix.net/registration/

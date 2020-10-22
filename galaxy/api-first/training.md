# Postman Galaxy 2020 - API First Training

This directory includes supporting resources for the API First training session at Postman Galaxy 2020! During the session you're going to work through generating collections from an API specification file that you'll import and edit.

<!--todo: api first intro-->

**Before you start, make sure you have the Postman agent installed–we're going to be working in [Postman on the web](http://go.postman.co/build/) (not the desktop app). Make sure you also create a new workspace for the session, so that you can share it later to request your API First credentials.**

<!--todo: share short url to this page-->

<!--todo: add toc-->

## 1. Import the starter spec

To get started, open the [customers](customers.yaml) (raw) file and download it. In Postman navigate to the new workspace you created for this session, choose **APIs** on the left, and click **+** to create a new API.

* 'Customers' as the name
* '1.0' as the version
* Import your downloaded spec file

**Save** the file–Postman will validate your schema and alert you to any issues within it.

> Try introducing a yaml syntax error into your spec file, or add an invalid element, to see how Postman highlights validation issues. Click the alert along the bottom to see more detail. As you edit the validation will update. The editor will also prompt you with element suggestions as you type.

You don't need to worry too much about the detail in the spec, but note the following:

* If you check out the base URL you'll see it's initially going to hit a mock API we set up in advance, but you'll be replacing this later with your own mock.
* It includes two initial endpoints, one for retrieving a customer, and one for adding a new customer.
* The request and response body schema are defined in the `components` at the end of the spec.
* The examples include dynamic variable references (with the syntax `{{$randomValue}}`) that Postman will use to generate random property values in the docs and mocks for the collections we generate.

Take a quick look at the tabs in the **API** builder–we'll get to know them better as we work through the session. We'll generate collections for a few different purposes including documentation and testing, all based on the spec, and link elements such as mock servers and monitors to it so that it becomes our single source of truth for the API.

## 2. Generate a documentation collection

Let's go ahead and generate a Postman collection from the spec. Click **Generate collection**. First let's create a collection we can use to work on documentation for the API.

* Enter the name 'Customer docs'
* Select **API Documentation**

> Take a quick look at the advanced options–we don't need them for now but you can tailor the detail of how your collections generate from the spec.

Click **Generate Collection** and Postman will add it in **Collections**. _If you click **View documentation** in the notification you can see the docs straight away._

Open the new collection and take a look at how the requests have been generated.

* By default your requests will be grouped in folders by path (you can change this to use tags from your spec).
* Your request names are pulled from the summaries in your spec.
* The URL indicated in the spec is stored as a collection level variable named `baseUrl`.
* The parameters, request bodies, and examples all populate from the examples in your spec (request details are auto-generated based on type if your spec doesn't include examples). _Since we used Postman dynamic variable references the examples will regenerate each time you send a mock request or view the docs._

Try sending the requests (make sure the desktop agent is selected). _Initially the requests are sending to the mock server we created before the session, but you'll send to your own mock later._

Postman will validate your collections against the linked spec as you work, but while you're in the request builder you'll only see an alert if your request is failing validation. Let's introduce an error intentionally so that we can see the validation in action.

* In the `POST` request, edit the **Body** to remove the `id`, which is flagged as required in the spec and will therefore make the response body invalid if it isn't present.
* **Save** the request–you should see a warning about the issue next to the request name. Click it to see further detail.
* Reinstate the `id` that you removed and **Save** the request.

Let's have a look at how the spec information populates into the documentation for the collection. With a request open, click the little docs icon at the top right.

Postman automatically indicates your request and response details including parameters, bodies, headers, and auth. You can add details by editing the information inline–edit a request description (you can use markdown) and save it. Click the link to view the complete docs to see how the collection as a whole is represented in docs form.

Check out the examples in your collection docs parameters and request / response bodies–they use the examples from your spec. Since we used the Postman dynamic variable syntax in the spec, your examples will dynamically generate random values whenever your docs are viewed. Try closing the docs tab and opening again to see this in action.

> The example name for a response comes from your API spec description for the response. If you have more than one response listed for a request, Postman will generate an example for each one, and your docs will present them via a drop-down list.

Pop back into the Customers API and open the **Develop** tab. Your linked docs collection will be listed. Click to validate it–it should be valid.

* Make your collection fail validation again by going back into the `POST` request and removing the `id` from the **Body** like you did before, and **Save** the request.
* Back in the API, click to validate again–this time you should see that issues were found.
* Click the validation warning and choose **Review details**.
* Postman will suggest changes to make your collection valid. **Make all changes to the request** and **Confirm**.

Return to the workspace and API. We will add more linked collections and other elements to the API as we work through the session.

## 3. Add a new endpoint

Let's make a change to the spec. We have endpoints for adding and retrieving a customer, but we need one for retrieving a list of customers.

In **APIs** &gt; **Customers** &gt; **Define**. Add a new endpoint inside `paths`, after the `post` request and before the `components` (making sure it's indented from `paths`).

```yaml
  /customers:
    get:
      summary: Retrieve details for all customers
      operationId: listCustomers
      tags:
          - customer
      responses:
          '200':
              description: Details of all customers
              content:
                  application/json:
                      schema:
                          $ref: '#/components/schemas/CustomerList'
```

The endpoint is going to be at the path `/customers` and will return a list of customer objects (referencing the existing `Customer` schema). Add the `CustomerList` schema in `components` at the end of the file.

```yaml
    CustomerList:
      type: array
      items:
        $ref: '#/components/schemas/Customer'
```

**Save** the spec. Back in **Develop**, validate the linked docs collection again. Click to **Review Issues**. Select the suggested changes and confirm, then navigate back to the workspace / collection to see the new endpoint.

## 4. Create a mock collection

Next we're going to create a collection we can use with a mock server we're also going to create. In the spec, change the URL to reference a variable as follows:

```yaml
  - url: '{{url}}'
```

When we create the mock server, its address will be stored in a variable with this name, and we will be able to switch between the original server and the new mock using an environment.

**Save** the spec and **Generate Collection** again, this time choosing **API Mocking**. Give your collection the name 'Customer mocks' and **Generate** it.

Name your mock server 'Mock customers' and check the box to save the URL as an environment variable. Postman will create a new environment for the mock as well as generating the mock and collection.

Select the new environment, it will have the same name as the mock server: 'Mock customers'. Click the eye button to see that it has a `url` variable with the address of your new mock server.

Select the `Customer mocks` collection and open the collection **Variables**. The `baseUrl` references the `url` variable you added to the spec, which will now have the value in the selected environment.

> You could have either a collection variable or another environment variable named `url` with another location, e.g. for your prod API, selecting and deselecting environments to switch between them with the same requests. Any environment var will override a collection var of the same name.

<!--todo: optionally go back into docs collection, add a url var to point to the original mock and make baseurl point to {{url}} so that with no env selected it hits original collection var url-->
<!--todo: link environment to api-->

**Send** one of the requests in the mock collection. This time it will hit the new mock you created (you can check where it sent in the **Console**).

You can edit your examples, for example if you prefer not to use the dynamic variables, or if you want to use a different one. Let's edit an example in the new mock collection to make it invalid.

* Open the example for the `GET` request that retrieves a single specific customer.
* In the response body, remove the "name" property and **Save**.

In the API **Develop** tab, validate the linked mock. Click to review the issues and make the suggested changes, confirming and returning to the workspace / spec. _Make sure you reselect the mocks environment whenever you leave and return to the workspace._

Back in the spec, let's make a change to the examples and see how that propagates to the collection. Add a new property to the `Confirm` schema and make it required, so that the whole schema looks like this:

```yaml
    Confirm:
      type: object
      required:
        - message
        - status
      properties:
        message:
          type: string
          example: 'New customer added!'
        status:
            type: string
            example: 'OK'
```

**Save** the spec and go back into the `POST` request in the mock collection–**Send** it. An issue will appear–click to see more detail.

Go back into the API **Develop** tab and validate the mock collection again, reviewing the issues, making the changes, and navigating back to the workspace / collection (selecting the mock environment again). **Send** the mocks `POST` request again–the new property should be returned.

## 5. Add a test suite

<!--todo: contracts intro-->

The final collection we're going to generate is for testing. Back in the API, hit **Generate Collection** again, this time choosing **Contract Testing**, with the name 'Customer contract tests'.

In the new collection, open the `GET` request that returns a particular customer. In the **Tests** tab for the request, add a test from the snippets on the right (click **<** if they don't display by default). Add **Status code: Code is 200** to the tests, **Save**, and **Send** the request. The test should pass.

Add the same snippet to the other two requests, but in the `POST` request change the code to `201` as follows:

```js
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});
```

All of your requests should now have a test of some kind in them–to add one that looks a little more like a real contract test let's define a schema and validate against it in the `GET` request that retrieves a particular customer.

```js
const schema = {
  "properties": {
    "id": {
        "type": "integer"
    },
    "company": {
        "type": "string"
    },
    "name": {
        "type": "string"
    }
  }
};
pm.test('Schema is valid', function() {
  pm.response.to.have.jsonSchema(schema);
});
```

**Save** and **Send** the request–it should pass. Try making it fail by changing the `type` for `id` to `boolean`.

> Tip: leave the test failing so that you see more interesting results when you add a monitor next.

<!--todo: maybe run once in collection runner-->

## 6. Add a monitor

Finally, let's monitor the API. In the API **Observe** tab, click **Add monitor** &gt; **Create new monitor**. Click **Use existing collection** and choose the 'Customer contract tests' collection.

> A monitor is the same as using the collection runner, or Newman. It runs your collection on a schedule and alert you to any failed tests by notification email.

Give your monitor the name 'Monitor customers', select the 'Mock customers' environment, choose a frequency, and create your monitor.

Click the link to open the monitoring page in the web dashboard. Rather than waiting for the scheduled time, hit **Run**! There will be a short delay while your monitor runs but when it completes you will see an overview of the test results.

Navigate back to the workspace / API and click the monitor name in **Observe**. You will see an overview of the monitor runs. Click a run and scroll down to see the detail of any test fails. You can filter the results and drill down to individual requests.

<!--todo: show commenting, sharing, mention reporting-->
<!--todo: make workspace public, talk through explore, link to submission-->
<!--todo: follow up resources-->
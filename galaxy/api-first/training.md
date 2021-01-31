# Postman Galaxy 2021 - API First Training

This API includes supporting resources for the API First training session at Postman Galaxy 2021! During the session you're going to work through generating collections from this API specification.

On completion of your submission for this training, you will receive the [Postman API First](https://badgr.com/public/badges/4ZMWYUmITU20Lf9JdAR6Xw) badge!

> 📌 Adopting a best practice approach to your API development process isn't as much of a hurdle as you might expect. By starting from an OpenAPI specification in Postman, you can generate multiple other components within a robust framework that delivers reliability and performance. Having a spec act as your single source of truth sets your API product or project up for governance, maintainability, effective adoption, and ultimately a competitive advantage.

**Before you start, make sure you have the Postman agent installed–we're going to be working in [Postman on the web](http://go.postman.co/build/) (not the desktop app). Make sure you also create a new workspace for the session–and make sure you choose a Public workspace, so that you can share it later, forwarding the link to request your API First certification.**

## The Customers specification

Navigate to the public workspace for the session: [bit.ly/postman-training](http://bit.ly/postman-training)

In __APIs__, select __Customers__. Select the __Define__ tab.

You don't need to worry too much about the detail in the spec, but note the following:

* If you check out the base URL you'll see it's initially going to hit a mock API we set up in advance, but you'll be replacing this later with your own mock.
* It includes two initial endpoints, one for retrieving a customer, and one for adding a new customer.
* The request and response body schema are defined in the `components` at the end of the spec.
* The examples include dynamic variable references (with the syntax `{{$randomValue}}`) that Postman will use to generate random property values in the docs and mocks for the collections we generate.

Take a quick look at the tabs in the **API** builder–we'll get to know them better as we work through the session. We'll generate collections for a few different purposes including documentation and testing, all based on the spec, and link elements such as mock servers and monitors to it so that it becomes our single source of truth for the API. Notice that you can comment and share the API to collaborate with teammates–Postman can also generate reports on your API to give stakeholders an overview of how it's performing.

## 1. Import the starter spec

In the **Define** tab &gt; **Copy** the content of the specification using the button at the top right of the edit area (you can toggle back and forth between it and the API __Overview__ here).

In the new workspace you created for this session (you might want to keep this tab open as well so that you can refer to the info here), choose **APIs** on the left, and click **+** to create a new API.

* `Customers` as the name.
* `1.0` as the version.
* `OpenAPI 3.0` as the schema type.
* `JSON` as the schema format.
* Create your new API and paste in the content you copied (replace the default contents).

> You can sync your Postman API with a linked spec, for example via GitHub integration.

**Save** the API–Postman will validate your schema and alert you to any issues within it. Open the __Define__ tab to see the specification JSON.

> 📌 Try introducing a syntax error into your spec file, or add an invalid element, to see how Postman highlights validation issues. Click the alert along the bottom to see more detail. As you edit, the validation will update. The editor will also prompt you with element suggestions as you type.

## 2. Generate a documentation collection

Let's go ahead and generate a Postman collection from the spec. Click **Generate collection**. First let's create a collection we can use to work on documentation for the API.

* Enter the name `Customer docs`.
* Select **API Documentation**.

> 📌 Take a quick look at the advanced options–we don't need them for now but you can tailor the detail of how your collections generate from the spec.

Click **Generate Collection** and Postman will add it in **Collections**. _If you click **View documentation** in the notification you can see the docs straight away._ **Keep the complete documentation view open in a tab as you work through the steps.**
<br/><br/>
Open the new collection and take a look at how the requests have been generated.

<ul>
    <li>By default your requests will be grouped in folders by path (you can change this to use tags from your spec
        using the advanced settings).</li>
    <li>Your request names are pulled from the summaries in your spec.</li>
    <li>The URL indicated in the spec is stored as a collection level variable named <code>baseUrl</code>.</li>
    <li>The parameters, request bodies, and examples all populate from the examples in your spec (request details are
        auto-generated based on type if your spec doesn&#39;t include examples). <em>Since we used Postman dynamic
            variable references, the examples will regenerate each time you send a mock request or view the docs.</em>
    </li>
</ul>
<p>Try sending the requests (make sure the desktop agent is selected). <em>Initially the requests will send to the mock
        server we created before the session, but you&#39;ll send to your own mock later.</em></p>
<h3 id='validate'>Validate</h3>
<p>Postman will validate your collections against the linked spec as you work, but while you&#39;re in the request
    builder you&#39;ll only see an alert if your request is failing validation. Let&#39;s introduce an error
    intentionally so that we can see the validation in action.</p>
<ul>
    <li>In the <code>POST</code> request, edit the <strong>Body</strong> to remove the <code>id</code>, which is flagged
        as required in the spec and will therefore make the response body invalid if it isn&#39;t present.</li>
    <li><strong>Save</strong> the request–you should see a warning about the issue next to the request name. Click it to
        see further detail.</li>
    <li>Reinstate the <code>id</code> that you removed and <strong>Save</strong> the request.</li>
</ul>
<h3 id='view-the-docs'>View the docs</h3>
<p>Let&#39;s have a look at how the spec information populates into the documentation for the collection. With a request
    open, click the little docs icon at the top right.</p>
<p>Postman automatically indicates your request and response details including parameters, bodies, headers, and auth.
    You can add details by editing the information inline–edit a request description (you can use markdown) and save it.
    Click the link to view the complete docs to see how the collection as a whole is represented in docs form.</p>
<p>Check out the examples in your collection docs parameters and request / response bodies–they use the examples from
    your spec. Since we used the Postman dynamic variable syntax in the spec, your examples will dynamically generate
    random values whenever your docs are viewed. Try closing the docs tab and opening again to see this in action.</p>
<blockquote>
    <p>📌 The example name for a response comes from your API spec description for the response. If you have more than
        one response listed for a request, Postman will generate an example for each one, and your docs will present
        them via a drop-down list.</p>
</blockquote>
<p>Pop back into the Customers API and open the <strong>Develop</strong> tab. Your linked docs collection will be
    listed. Click to validate it–it should be valid.</p>
<ul>
    <li>Make your collection fail validation again by going back into the <code>POST</code> request and removing the
        <code>id</code> from the <strong>Body</strong> like you did before, and <strong>Save</strong> the request.</li>
    <li>Back in the API, click to validate again–this time you should see that issues were found.</li>
    <li>Click the validation warning and choose <strong>Review details</strong>.</li>
    <li>Postman will suggest changes to make your collection valid. <strong>Make all changes to the request</strong> and
        <strong>Confirm</strong>.</li>
</ul>
<p>We will add more linked collections and other elements to the API as we work through the session.</p>
<h2 id='3-add-a-new-endpoint'>3. Add a new endpoint</h2>
<p>Let&#39;s make a change to the spec. We have endpoints for adding and retrieving a customer, but we need one for
    retrieving a list of customers.</p>
<p>In <strong>APIs</strong> &gt; <strong>Customers</strong> &gt; <strong>Define</strong>. Add a new endpoint inside
    <code>paths</code>, after the <code>post</code> request (adding a comma before the previous element–you can hit the
    <strong>Beautify</strong> button at the top right to clean up your indentation).</p>
<pre><code>"/customers": {
    "get": {
        "summary": "Retrieve details for all customers",
        "operationId": "listCustomers",
        "tags": [
            "customer"
        ],
        "responses": {
            "200": {
                "description": "Details of all customers",
                "content": {
                    "application/json": {
                        "schema": {
                            "$ref": "#/components/schemas/CustomerList"
                        }
                    }
                }
            }
        }
    }
}</code></pre>
<p>The endpoint is going to be at the path <code>/customers</code> and will return a list of customer objects
    (referencing the existing <code>Customer</code> schema). Add the <code>CustomerList</code> schema in
    <code>components</code> &gt; <code>schemas</code> after the existing schema elements.</p>
<pre><code>"CustomerList": {
    "type": "array",
    "items": {
        "$ref": "#/components/schemas/Customer"
    }
}</code></pre>
<p><strong>Save</strong> the spec. Back in <strong>Develop</strong>, validate the linked docs collection again. Click to
    <strong>Review Issues</strong>. Select the suggested changes and confirm, then navigate back to the collection to
    see the new endpoint.</p>
<h2 id='4-create-a-mock-collection'>4. Create a mock collection</h2>
<p>Next we&#39;re going to create a collection we can use with a mock server we&#39;re also going to create.
<p>When we create the mock server, its address will be stored in a variable, and we will be able to switch between the
    original server and the new mock using an environment.</p>
<p><strong>Save</strong> the spec and <strong>Generate Collection</strong> again, this time choosing <strong>API
        Mocking</strong>. Give your collection the name <code>Customer mocks</code> and <strong>Generate</strong> it.
</p>
<p>Name your mock server <code>Mock customers</code> and check the box to save the URL as an environment variable.
    Postman will create a new environment for the mock as well as generating the mock and collection.</p>
<h3 id='use-environments'>Use environments</h3>
<p>Select the new environment, it will have the same name as the mock server: <code>Mock customers</code>. Click the eye
    button to see that it has a <code>url</code> variable with the address of your new mock server–<strong>edit the
        variable name, changing it to <code>baseUrl</code></strong>.</p>
<p>Select the <code>Customer mocks</code> collection and open the collection <strong>Variables</strong>. The
    <code>baseUrl</code> includes the original URL from the spec, but by selecting the environment, the requests will
    reference the mock URL instead, because Postman scope means that the environment value overrides the collection
    value.</p>
<blockquote>
    <p>📌 You can also link Environments to a spec in the API <strong>Develop</strong>tab.</p>
</blockquote>
<p><strong>Send</strong> one of the requests in the mock collection. This time it will hit the new mock you created (you
    can check where it sent in the <strong>Console</strong>).</p>
<h3 id='author-examples'>Author examples</h3>
<p>You can edit your examples, for example if you prefer not to use the dynamic variables, or if you want to use a
    different one. Let&#39;s edit an example in the new mock collection to make it invalid.</p>
<ul>
    <li>Open the example for the <code>GET</code> request that retrieves a single specific customer.</li>
    <li>In the response body, remove the &quot;name&quot; property and <strong>Save</strong>.</li>
</ul>
<p>In the API <strong>Develop</strong> tab, validate the linked mock. Click to review the issues and make the suggested
    changes, confirming and returning to the workspace / spec. <em>Make sure you reselect the mocks environment if you
        leave and return to the workspace.</em></p>
<h3 id='edit-the-spec'>Edit the spec</h3>
<p>Back in the spec, let&#39;s make a change to the examples and see how that propagates to the collection. Add a new
    <code>status</code> string property to the <code>Confirm</code> schema and make it required, so that the whole
    schema looks like this:</p>
<pre><code>"Confirm": {
    "type": "object",
    "required": [
        "message",
        "status"
    ],
    "properties": {
        "message": {
            "type": "string",
            "example": "New customer added!"
        },
        "status": {
            "type": "string",
            "example": "OK"
        }
    }
},</code></pre>
<p><strong>Save</strong> the spec and go back into the <code>POST</code> request in the mock
    collection–<strong>Send</strong> it.</p>
<p>Go back into the API <strong>Develop</strong> tab and validate the mock collection again, reviewing the issues,
    making the changes, and navigating back to the collection. <strong>Send</strong> the mocks <code>POST</code> request
    again–the new property should be returned.</p>
<h2 id='5-add-a-test-suite'>5. Add a test suite</h2>
<p>Combining test suites with an API-spec driven workflow builds a level of consistency and compliance into your API
    development and deployment pipeline.</p>
<p>The final collection we&#39;re going to generate is for testing. Back in the API, hit <strong>Generate
        Collection</strong> again, this time choosing <strong>Contract Testing</strong>, with the name
    <code>Customer contract tests</code>.</p>
<p>In the new collection, open the <code>GET</code> request that returns a particular customer. In the
    <strong>Tests</strong> tab for the request, add a test from the snippets on the right (click <strong>&lt;</strong>
    if they don&#39;t display by default). Add <strong>Status code: Code is 200</strong> to the tests,
    <strong>Save</strong>, and <strong>Send</strong> the request. The test should pass.</p>
<p>Add the same snippet to the other two requests, but in the <code>POST</code> request change the code to
    <code>201</code> as follows:</p>
<pre><code>pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});</code></pre>
<p>All of your requests should now have a test of some kind in them–to add one that looks a little more like a real
    contract test let&#39;s define a schema and validate against it in the <code>GET</code> request that retrieves a
    particular customer.</p>
<pre><code>const schema = {
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
pm.test("Schema is valid", function() {
  pm.response.to.have.jsonSchema(schema);
});</code></pre>
<p><strong>Save</strong> and <strong>Send</strong> the request–it should pass. Try making it fail by changing the
    <code>type</code> for <code>id</code> to <code>boolean</code>.</p>
<blockquote>
    <p>📌 Tip: leave the test failing so that you see more interesting results when you add a monitor next.</p>
</blockquote>
<p>Add a final test to the <strong>customer</strong> folder–this will run for every request inside the folder. In the
    <strong>Tests</strong> for the folder, add the following to test the response time:</p>
<pre><code>pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});</code></pre>
<p>Try running your collection in the collection runner–select the collection and hit <strong>Run</strong>, running with
    the default options. Check out the output, selecting requests to drill down into the detail (which you can also do
    in the <strong>Console</strong>).</p>
<h2 id='6-add-a-monitor'>6. Add a monitor</h2>
<p>Finally, let&#39;s monitor the API. In the API <strong>Observe</strong> tab, click <strong>Add monitor</strong> &gt;
    <strong>Create new monitor</strong>. Click <strong>Use existing collection</strong> and choose the
    <code>Customer contract tests</code> collection.</p>
<blockquote>
    <p>📌 A monitor is similar to using the collection runner, or Newman. It runs your collection on a schedule and
        alerts you to any failed tests by notification email.</p>
</blockquote>
<p>Give your monitor the name <code>Monitor customers</code>, select the <code>Mock customers</code> environment, choose
    a frequency, and create your monitor.</p>
<p>Rather than waiting for the scheduled time, hit
    <strong>Run</strong>! There will be a short delay while your monitor runs but when it completes you will see an
    overview of the test results.</p>
<p>Navigate back to the API and click the monitor name in <strong>Observe</strong>. You will see an overview
    of the monitor runs. Click a run and scroll down to see the detail of any test fails. You can filter the results and
    drill down to individual requests.</p>
<h2 id='7-complete-your-submission'>7. Complete your submission</h2>
<p>Once you have all of your collections generated from your spec as outlined above and you&#39;re happy with your
    workspace, you can go ahead and share it (making sure it's set to <strong>Public</strong> in the workspace
    overview), then share the link to get your API First badge and swag!</p>
<p>When your workspace is public, anyone can open it in the browser and fork your collections to edit or send the
    requests in them. You can copy the workspace link from your browser address bar after making it public.</p>
<blockquote>
    <p>📌 Note that a Postman team admin can set the account to require approval to make a workspace public via the
        <strong>Community Manager</strong> role.</p>
</blockquote>
<p>Fill out the form <a href='https://bit.ly/submit-api-first'>bit.ly/submit-api-first</a> (if anything is missing we will
    follow up with you).</p>

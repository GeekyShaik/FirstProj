1 pm.sendRequest("https://wsdevinternal.usaa.com/tokens/v1/id-claim?party_id=PLY2816", function (err, response) {

2

var jsonData = response.json();

3 pm.environment.set("idClaim", jsonData['id-claim']);

});
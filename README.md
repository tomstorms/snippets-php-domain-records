# snippets-php-domain-records


# tomstorms / Snippets / PHP Domain Records

Snippet code demonstrating how you could potentially do domain/DNS verification checks in PHP.

Example:

In this example, we're looking at the domain ```www.islandmeetscity.com``` for a ```TXT``` record with the value ```kboZ2tg22FjOPUJQliMJ4QW5g```. The ```isDomainVerified``` function will return true/false if this record exists.

```
<?php

$domainName = 'www.islandmeetscity.com';
$requiredTXTValue = 'kboZ2tg22FjOPUJQliMJ4QW5g';

$successful = isDomainVerified($domainName, $requiredTXTValue);

// ==========

function isDomainVerified($domainName, $matchingValue, $recordType='TXT') {
	if (!$domainName || $domainName == '') return;
	if (!$matchingValue || $matchingValue == '') return;
	if ($recordType == '') return;

	$successful = false;

	$dnsRecords = dns_get_record($domainName, DNS_ALL - DNS_PTR);

	foreach($dnsRecords as $dnsRecord) {
		if (strtolower(trim($dnsRecord['type'])) == strtolower(trim($recordType))) {
			if (isset($dnsRecord['entries'][0])) {

				$txtValue = $dnsRecord['entries'][0];

				$successful = ($txtValue == $matchingValue);

				if ($successful) break;
			}
		}
	}

	return $successful;
}

// ==========

echo 'Is successful? '.($successful ? 'Yes' : 'No').'<br/>';

echo 'Discovered domain records:';
$dnsRecords = dns_get_record($domainName, DNS_ALL - DNS_PTR);
print_r($dnsRecords);

```

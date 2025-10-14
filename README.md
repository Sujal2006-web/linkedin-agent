# linkedin-agent
// {
  "name": "Ultimate Lnkdn email finder",
  "nodes": [
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.apify.com/v2/acts/2SyF0bVxmgGr8IVCZ/run-sync-get-dataset-items",
        "sendHeaders": true,
        "headerParameters": [
          {
            "name": "Accept",
            "value": "application/json"
          },
          {
            "name": "Authorization",
            "value": "Bearer XXXXXXXXXXXXXXXXXXXXXXXXX"
          }
        ],
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "{\n    \"profileUrls\": [\n        \"{{ $('Scraped Leads Data').item.json['Linkedin URL'] }}\"\n    ]\n}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        240,
        120
      ],
      "id": "5e686c3f-ee54-4c07-9b24-498832ad824a",
      "name": "HTTP Request2"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        20,
        100
      ],
      "id": "065a8e24-1dbb-4e03-82fa-4c121d126b79",
      "name": "Loop Over Items"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "8b0c89ca-b86f-4129-9a30-cf77e7097402",
              "name": "Full Name",
              "value": "={{ $json.fullName }}",
              "type": "string"
            },
            {
              "id": "efa1897b-da30-4bc1-bd00-3481b7992b30",
              "name": "Email",
              "value": "={{ $json.email }}",
              "type": "string"
            },
            {
              "id": "9080e0a4-db15-4457-8d42-92eb8b98d2e5",
              "name": "Company Name",
              "value": "={{ $json.companyName }}",
              "type": "string"
            },
            {
              "id": "0a3d7d55-c3c6-465a-b932-e6b1b23d53df",
              "name": "Company Website",
              "value": "={{ $json.companyWebsite }}",
              "type": "string"
            },
            {
              "id": "b949ff8c-d54c-4b63-9ca6-eb72ac6f9a6e",
              "name": "Role",
              "value": "={{ $json.experiences[0].title }}",
              "type": "string"
            },
            {
              "id": "ee0d838d-e2d7-47c5-afd9-014cf00b1297",
              "name": "Company Industry",
              "value": "={{ $json.companyIndustry }}",
              "type": "string"
            },
            {
              "id": "f62e4489-3af5-414e-a7eb-b3bf51e0cfdc",
              "name": "Location",
              "value": "={{ $json.addressWithCountry }}",
              "type": "string"
            },
            {
              "id": "3fd38f3f-db6b-4ccc-9aa0-06ad9393258f",
              "name": "linkedinUrl",
              "value": "={{ $json.linkedinUrl }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        440,
        120
      ],
      "id": "993d1746-bcc3-4960-864e-85c29b35d238",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "04d0bbba-615a-48d0-9391-93707d995f6f",
              "leftValue": "={{ $json.Email }}",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "exists",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        80,
        360
      ],
      "id": "5b64c909-c0f4-4d74-92dd-e0317860b325",
      "name": "If"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "123e70cc-ba68-4d07-8d44-760d410a670e",
              "leftValue": "={{ $json['Company Website'] }}",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "exists",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        320,
        600
      ],
      "id": "3872d4b9-7090-47ab-b3df-20e73ecac188",
      "name": "If1"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "loose",
            "version": 2
          },
          "conditions": [
            {
              "id": "06110187-014e-484a-a264-513f18a5098f",
              "leftValue": "={{ $json.emails }}",
              "rightValue": 0,
              "operator": {
                "type": "string",
                "operation": "notExists",
                "singleValue": true
              }
            },
            {
              "id": "aff917a0-9717-460b-89d6-492d435b3b82",
              "leftValue": "={{ $json.emails }}",
              "rightValue": "NIL",
              "operator": {
                "type": "string",
                "operation": "equals",
                "name": "filter.operator.equals"
              }
            }
          ],
          "combinator": "or"
        },
        "looseTypeValidation": true,
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        980,
        360
      ],
      "id": "023d9722-4e1d-49f2-bdab-e54c35defe95",
      "name": "If2"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        520,
        340
      ],
      "id": "1306657f-f056-42ae-a3e0-7a7a5c85bf2d",
      "name": "Wait",
      "webhookId": "445a3682-b83c-4573-bfbf-465933f8ba2b"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        1440,
        340
      ],
      "id": "51d0115d-05ce-4182-be74-2de9b6a9df7a",
      "name": "Wait3",
      "webhookId": "445a3682-b83c-4573-bfbf-465933f8ba2b"
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs",
          "mode": "list",
          "cachedResultName": "Linkdin Outreach (solar)",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1453550326,
          "mode": "list",
          "cachedResultName": "Scraped Leads - Google",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit#gid=1453550326"
        },
        "filtersUI": {
          "values": [
            {
              "lookupColumn": "Customer",
              "lookupValue": "Ideal"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.5,
      "position": [
        -180,
        100
      ],
      "id": "2620ea27-0c8a-4799-b97b-7c6cba784609",
      "name": "Scraped Leads Data",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "tY0FJBuJ7JTKB0im",
          "name": "N8n X Google"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs",
          "mode": "list",
          "cachedResultName": "Linkdin Outreach (solar)",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1763230603,
          "mode": "list",
          "cachedResultName": "Emails Enriched",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit#gid=1763230603"
        },
        "columns": {
          "mappingMode": "autoMapInputData",
          "value": {},
          "matchingColumns": [
            "row_number"
          ],
          "schema": [
            {
              "id": "Full Name",
              "displayName": "Full Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Email",
              "displayName": "Email",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Company Name",
              "displayName": "Company Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Company Website",
              "displayName": "Company Website",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Role",
              "displayName": "Role",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Company Industry",
              "displayName": "Company Industry",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "Location",
              "displayName": "Location",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "linkedinUrl",
              "displayName": "linkedinUrl",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.5,
      "position": [
        320,
        340
      ],
      "id": "4d895789-0539-4f4a-aca7-9044f77d90cc",
      "name": "Enriched Emails",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "tY0FJBuJ7JTKB0im",
          "name": "N8n X Google"
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs",
          "mode": "list",
          "cachedResultName": "Linkdin Outreach (solar)",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1453550326,
          "mode": "list",
          "cachedResultName": "Scraped Leads - Google",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hPaRoR-JZcldS_gfTm3_o146iNc9nKQF4l_V-svQ6qs/edit#gid=1453550326"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "row_number": "={{ $('Loop Over Items').first().json.row_number }}",
            "Status": "Enriched",
            "Customer": "Lead sent"
          },
          "matchingColumns": [
            "row_number"
          ],
          "schema": [
            {
              "id": "Name",
              "displayName": "Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "Role",
              "displayName": "Role",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "Linkedin URL",
              "displayName": "Linkedin URL",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "More info",
              "displayName": "More info

##############
GET jobs
##############

Lists all the member's jobs.  

Note that non-Forge jobs, i.e., jobs running on a 3D printer that was initiated outside of Forge, are integrated into the Forge job queue and assigned to the printer owner.

********************
Resource Information
********************

======================   ========================================================================
Method and URI           GET \https://developer.api.autodesk.com/ps/v1/print/jobs
Authentication Context   user context required 
Required OAuth Scopes    ``data:read`` ``data:write``
Data Format              JSON
======================   ========================================================================


*******
Request
*******

HTTP Headers
=============
 

=============   ========   ==========   =====================================================================================================
Name            Required   Value Type   Description
=============   ========   ==========   =====================================================================================================
Authorization   yes        string       Must be ``Bearer <token>``, where ``<token>`` is obtained via a `three-legged OAuth flow </en/docs/oauth/v2/tutorials/get-3-legged-token>`_
=============   ========   ==========   =====================================================================================================



 
Query String Parameters
=======================

==================   ===================   ================   ===================================================================
Name                 Required              Value Type         Description
==================   ===================   ================   ===================================================================
status               no                    array: string      | a JSON array of the statuses to include in the list
                                                              |
                                                              | Possible values are ``completed``, ``failed``, ``canceled``,
                                                                ``successful``, and ``queued``.
                                                              |
                                                              | If omitted, all jobs will be returned, regardless of status.
limit                no                    int                | maximum number of jobs to return per page
                                                              |
                                                              | If set to ``-1``, it will return all jobs. 
offset               no                    int                starting index of the current list of jobs
==================   ===================   ================   ===================================================================


********
Response
********


HTTP Status Code Summary
========================

====   =====================   =====================================================================================
Code   Message                 Meaning
====   =====================   =====================================================================================
200    OK                      Successful
400    Bad Request             Invalid request. E.g., the request could not be parsed.
401    Unauthorized            Invalid authorization header. E.g., user is not logged in, or cannot access resource.
404    Not Found               Endpoint does not exist.
422    Unprocessable Entity    Semantic errors
429    Precondition Required   Request rate limit exceeded
500    Internal Server Error   An unknown error occurred on the server.
====   =====================   =====================================================================================

Body Structure (200)
====================

A successful response returns a JSON object with the following attributes:

====================================  ======================  ============================================================================================
Attribute                             Value Type              Description
====================================  ======================  ============================================================================================
count                                 int                     the number of jobs returned
limit                                 int                     maximum number of jobs to return
offset                                int                     starting index of the current list of jobs
_link_prev                            string: URI path and    URI for the previous set of results, excluding scheme and hostname
                                      query string
_link_next                            string: URI path and    URI for the next set of results, excluding scheme and hostname
                                      query string
_link                                 string: URI path and    URI for the endpoint called, excluding scheme and hostname
                                      query string          
member_id                             string                  identifier of the 3-legged end user
jobs                                  array: object           the list of jobs
*jobs*.job_id                         string: UUID            ID of the job
*jobs*.job_name                       string                  human-readable name of the job
*jobs*.printer_id                     int                     ID of the printer
*jobs*.job_status                     object                  extended information about the status of the job
*jobs.job_status*.printer_status      enum: string            | status of the printer
                                                              |
                                                              | See the `Firmware Workflow </en/docs/print/v1/overview/firmware-workflow>`_
                                                                page for more information about statuses. 
*jobs.job_status*.job_id              string: UUID            ID of the job
*jobs.job_status*.job_name            string                  human-readable name of the job
*jobs.job_status*.member_id           string                  ID of the member
*jobs.job_status*.printer_id          int                     ID of the printer
*jobs.job_status*.printer_name        string                  human-readable name of the printer
*jobs.job_status*.job_status          string                  | status of the job
                                                              |
                                                              | See the `Firmware Workflow </en/docs/print/v1/overview/firmware-workflow>`_
                                                                page for more information about statuses. 
*jobs*.job_data                       object                  printer specific data sent by the printer
*jobs.job_data*.printable_id          string: URN             URN of the file to be printed
*jobs*.job_date_time                  string                  date the job was created in ``<month_name> D, YYYY HH:mm:ss`` format
*jobs*.job_status_time                string                  date the last status was sent in ``<month_name> D, YYYY HH:mm:ss`` format
*jobs*.member_id                      string                  the ID of the member
*jobs*.local_job                      bool                    whether the job was created by print services or sent locally
*jobs*.status                         string                  | status of the job
                                                              |
                                                              | See the `Firmware Workflow </en/docs/print/v1/overview/firmware-workflow>`_
                                                                page for more information about statuses. 
====================================  ======================  ============================================================================================


**********
Examples
**********

Successful Retreival of Jobs (200)
==================================

.. code-block:: shell

  curl -X 'GET' -H 'Authorization: Bearer jwP63dl3zT4fSAiy6jOBrmcr1UKh' -v 'https://developer.api.autodesk.com/ps/v1/print/jobs'

.. code-block:: json

  {
    "count": 3,
    "limit": 20,
    "offset": 0,
    "_link_prev": "",
    "_link_next": "",
    "_link": "/print/jobs?limit=20&offset=0",
    "member_id": "FPEJMQHAG3NXE",
    "jobs": [
      {
        "job_id": "4b5a3181-8997-4d6a-979f-e1af7eb5b545",
        "job_name": "my_job",
        "printer_id": 1092,
        "job_status": {
          "printer_status": "printing",
          "job_id": "4b5a3181-8997-4d6a-979f-e1af7eb5b545",
          "job_name": "my_job",
          "member_id": "FPEJMQHAG3NXE",
          "printer_id": 1092,
          "printer_name": "my_test_printer"
        },
        "job_data": {
          "printable_id": "urn:adsk.objects:os.object:print-services-external/b1996dd1-4b7f-44d8-9040-01615530073c.tar.gz"
        },
        "job_date_time": "May 18, 2016 05:42:02",
        "job_status_time": "May 18, 2016 08:59:59",
        "local_job": false
      },
      {
        "job_id": "d075f5ee-98b7-491a-b4b1-188260b01c59",
        "job_name": "my_job2",
        "printer_id": 1092,
        "job_status": {
          "job_status": "queued"
        },
        "job_data": {
          "printable_id": "urn:adsk.objects:os.object:print-services-external/b1996dd1-4b7f-44d8-9040-01615530073c.tar.gz",
          "printable_url": "https://developer.api.autodesk.com/oss/v2/signedresources/5d1f9e09-0eb9-43df-bdc6-9cdea0f0e65b"
        },
        "job_date_time": "May 18, 2016 06:01:12",
        "job_status_time": "May 18, 2016 06:01:12",
        "local_job": false,
        "status": "queued"
      },
      {
        "job_id": "37ef6f6a-abe0-43c7-a738-9c6fab4504d9",
        "job_name": "my_job3",
        "printer_id": 1093,
        "job_status": {
          "job_status": "queued"
        },
        "job_data": {
          "printable_id": "urn:adsk.objects:os.object:print-services-external/b1996dd1-4b7f-44d8-9040-01615530073c.tar.gz",
          "printable_url": "https://developer.api.autodesk.com/oss/v2/signedresources/6e47bd27-f96c-418e-834d-604c3ca9143c"
        },
        "job_date_time": "May 18, 2016 12:26:32",
        "job_status_time": "May 18, 2016 12:26:42",
        "local_job": false,
        "status": "queued"
      }
    ]
  }

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


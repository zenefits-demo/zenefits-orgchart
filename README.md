# zenefits-orgchart
This repo hosts the code to show the orgchart from the Zenefits platform

# Techstack

* Nodejs/Express for backend.
* [zenefits-demo/zf](https://github.com/zenefits-demo/zf) client SDK. This is used to interact with the Zenefits platform using the REST APIs.
* React JS and foundation for front end.


# Backend
1. Create integration with Zenefits platform either by 
    * Creating a custom integration: Non OpenId based authentication using just TOKENS.
    * Registering your service with Zenefits platform with URL details, token/claims to fetch.
2. Create a wrapper around SDK client and add caching ability
    * Refresh the local cache every ~10mins. (Setup a timer). Use Redis-cache for scalability if needed. 
    * If the entry doesn't exist, fetch "all the data" and then return the
3. APIs:
    * Company API: Fetch the company details.
    * Employee API: Get the tree on the employees starting from the CEO/Head (one who doesn't have any manager).
     ```{
          Display name: "Peter White",
          designation: "CEO"
          totalReporteee: 30,
          directoReportees: 5,
          <<...other optional info like department, email, photo etc. >>
          reportees: {
            employee: "Matt Penn",
            designation: "VP",
            .
          }
     }```
4. Flow.
     - When the user lands on the homepage. Fetch the company details and employee details. 
         * For a marketplace app, we can ask the user to login to zenefits portal and fetch "their" company and details.
         * For a third-party/custom integration all the information is embedded in the token itself. So fetch the default details available
     - Show the top level employees and their direct/indirect reportee count.
     - When a user expands on any of particular employee, fetch the reportee information on-demand (lazily and cache it).
     - Adjust the UI layout according to the 
     

# Client side:
- Resonsive UI with foundation 6. 
- React/Redux (can go for simple Context API)
- Component hirearchy.
  - PageLayout.
    - Company
    - Employees
      - Employee
        - Employees (recurssive implementation for reportees)
    

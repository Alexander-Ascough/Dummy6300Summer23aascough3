# Requirements

> 1.	When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet).

The application begins with a class to represent the application, App() containing a single public method start(). 

That point of entry will immediately instantiate Main() and execute the public method getUserCommand_(), which is a command-line style interface intended to access the otherwise private methods, setCurrentJob(), setJobOffer(), setComparisonSettings(), and compareJobOffers(). For example, "Enter 1 to set current job, 2 to set job offer, 3 to adjust comparison settings, 4 to compare job offers." If no job offers have been entered, then compareJobOffers() will not appear in the prompt.

> 2.	When choosing to enter current job details, a user will:
> 	2.1.	Be shown a user interface to enter (if it is the first time) or edit all the details of their current job, which consist of:
> 		2.1.1.	Title
> 		2.1.2.	Company
> 		2.1.3.	Location (entered as city and state)
> 		2.1.4.	Cost of living in the location (expressed as an index)
> 		2.1.5.	Yearly salary
> 		2.1.6.	Yearly bonus
> 		2.1.7.	Leave (number of whole days from 0 to 30 inclusive)
> 		2.1.8.	Maternity/Paternity Leave (number of whole days from 0 to 20 inclusive)
> 		2.1.9.	Life Insurance (Percentage of Salary as an integer: 0 - 10)

Jobs are represented by the Job() class object. Because the current job and job offers are so similar, I have decided against polymorphism in favor of a simple public Boolean property, trueIfCurrent, to differentiate. Job() also contains several private properties corresponding to requirements 2.1.1 - 2.1.9, as well as public getter and setter methods in the traditional Java style. Each getter and setter will ensure proper data formatting as described in requirements, e.g. getLocation(city, state). Finally, these getters and setters shall interface with the user via Main()'s setCurrentJob(). 
	
> 2.b.	Be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.

Main()'s setCurrentJob() will offer these options.

> 3.	When choosing to enter job offers, a user will:
	> 	3.1.	Be shown a user interface to enter all the details of the offer, which are the same ones listed above for the current job.
	> 	3.2.	Be able to either save the job offer details or cancel.
	> 	3.3.	Be able to (1) enter another offer, (2) return to the main menu, or (3) compare the offer (if they saved it) with the current job details (if present).

Main()'s setJobOffer() will function the same as setCurrentJobs() as described above, thus fulfilling all these requirements, except Job()'s trueIfCurrent Boolean will be set to FALSE.
> 4.	When adjusting the comparison settings, the user can assign integer weights to:
> 4.1.	Yearly salary 
> 4.2.	Yearly bonus 
> 4.3.	Leave  
> 4.4.	Maternity/Paternity Leave 
> 4.5.	Life Insurance  (If no weights are assigned, all factors are considered equal.)

Comparator() class object contains private properties corresponding to requirements 4.1-4.5, as well as setters and getters in the traditional Java style.

> 5.	When choosing to compare job offers, a user will:
> 5.1.	Be shown a list of job offers, displayed as Title and Company, ranked from best to worst (see below for details), and including the current job (if present), clearly indicated.

Upon selecting Main()'s compareJobOffers() via getUserCommand(), Comparator()'s getListOfJobOffers() shall loop through the jobs Dictionary, which contains all jobs created so far, and concatenate a String satisfying requirement 5.1.

> 5.2. Select two jobs to compare and trigger the comparison.
> 5.3. Be shown a table comparing the two jobs, displaying, for each job:
> 5.3.1. Title
> 5.3.2. Company
> 5.3.3. Location 
> 5.3.4. Yearly salary adjusted for cost of living
> 5.3.5. Yearly bonus adjusted for cost of living
> 5.3.6. Leave
> 5.3.7. Maternity/Paternity Leave 
> 5.3.8. Life Insurance  
> 5.4 Be offered to perform another comparison or go back to the main menu.

Comparator()'s getJobComparison(jobA : Job, jobB : Job) will accept two job selections from the user, concatenate the table described in requirements 5.3 - 5.3.8, and then offer to return to Main()'s getUserCommand().

> 6.	When ranking jobs, a job’s score is computed as the weighted sum of: 
> AYS + AYB + (LT * AYS / 260) + (MPL * AYS / 260) + (LI/100 * AYS)
> where: AYS = yearly salary adjusted for cost of living AYB = yearly
> bonus adjusted for cost of living LT = Leave MPL = Maternity/Paternity
> Leave LI = Life Insurance For example, if the weights are 3 for the
> yearly salary, 2 for Maternity/Paternity Leave and 1 for all other
> factors, the score would be computed as: (3/7 * AYS) + (1/7 * AYB) +
> (1/7 * LT * AYS / 260) + (2/7 * MPL * AYS / 260) + (1/7* LI/100 * AYS)

Job()'s getRank() will compute this figure and output it as a Double (likely necessary due to nested division) for concatenation in Comparator()'s getListOfJobOffers().  

> 7.	The user interface must be intuitive and responsive.

I've written my architecture with a command-line style interface in mind because, provided clearly written prompts, this is arguably the simplest and most responsive interface possible. However, I recognize modern users may be more familiar with a pushbutton GUI, so I have omitted any explicit reference to command-line and kept the architecture flexible enough to fit in other architectures.

> 8.	For simplicity, you may assume there is a single system running the app (no communication or saving between devices is necessary).

Duly noted. This requirement doesn't add anything to the class architecture, but precludes the need for additional communication objects. 

# PlantUML Syntax, incl. Reqs Comments

Throughout the syntax used to generate my UML, I have included comments tracing design elements back to requirements. Included below, for your consideration.

@startuml

'Req#1'
class App{
  'Req#1'
  +void start()
}

'Req#1'
class Main{
  'Req Implied'
  +myComparator : Comparator
  'Req#1.1, 3.c'
  'Facilitates access to private methods'
  +void getUserCommand()
  'Req#1.1, 2'
  -void setCurrentJob()
  'Req#1.2, 3'
  -void setJobOffer()
  'Req#1.3, 4'
  -void setComparisonSettings()
  'Req#1.4, 5'
  -void compareJobOffers()
}

'Req#1.1-1.2"
class Job{
  'Req implied'
  +trueIfCurrent : Boolean
  'Req#2.a'
  -title : String 
  -company : String 
  -city : String
  -state : String 
  -yearlySalary : Integer
  -yearlyBonus : Integer
  -leave : Integer
  -parentalLeave : Integer
  -lifeInsurance : Integer
  +String getTitle()
  +String getCompany()
  +String getCity()
  +String getState()
  +Integer getYearlySalary()
  +Integer getYearlyBonus()
  +Integer getLeave()
  +Integer getParentalLeave()
  +Integer getLifeInsurance()
  +String getLocation(city, state)
  'Req#6'
  +Double getRank()
  +void setTitle()
  +void setCompany()
  +void setCity()
  +void setState()
  +void setYearlySalary()
  +void setYearlyBonus()
  +void setLeave()
  +void setParentalLeave()
  +void setLifeInsurance()
  'Req#3.a, 2.a'
  'Req#2.b: Be able to either save the job offer details or cancel.'
  'Req#3.b: b.	Be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.'
  +void getEditingInterface()
}

'Req#1.3-1.4'
class Comparator{
  'Req Implied'
  -jobs : Dictionary
  'Req#4.a'
  -yearlySalaryWeight : Integer
  -yearlyBonusWeight : Integer
  -leaveWeight : Integer
  -parentalLeaveWeight : Integer
  -lifeInsuranceWeight : Integer
  +Integer getYearlySalaryWeight()
  +Integer getYearlyBonusWeight()
  +Integer getLeaveWeight()
  +Integer getParentalLeaveWeight()
  +Integer getLifeInsuranceWeight()
  +void setYearlySalaryWeight()
  +void setYearlyBonusWeight()
  +void setLeaveWeight()
  +void setParentalLeaveWeight()
  +void setLifeInsuranceWeight()
  'Req#5.a:	Be shown a list of job offers, displayed as Title and Company, ranked from best to worst (see below for details), and including the current job (if present), clearly indicated.'
  +String getListOfJobOffers()
  'Req#5.b: Select two jobs to compare and trigger the comparison.'
  'Req#5.c-5.d: (See reqs document for detailed list of output reqs)
  +String getJobComparison(jobA : Job, jobB : Job)
  'Req#6'
  -double getWeightedSum()
  
}

'Req#1.1'
App --> Main
'Req (Implied)'
Main --> Job
Main --> Comparator
Comparator *-- Job

@enduml

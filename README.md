# Expert-Systems
This repository is all about Expert Systems and their programming languages such as Clips and building interface to them.


Building interface for expert-system programs written in Clips with Java


Gathered by:
Abdolrahman Farshgar
B.D of Software Engineering
Email Address: rahman.fa@live.com
Instagram: @rahman__94


About CLIPS
It was developed at NASA's Johnson Space Center from 1985 to 1996, the 'C' Language Integrated Production System (CLIPS) is a rule-based programming language useful for creating expert systems and other programs where a heuristic solution is easier to implement and maintain than an algorithmic solution. Written in C for portability, CLIPS can be installed and used on a wide variety of platforms. Since 1996, CLIPS has been available as public domain software.
The Clips  programming language is a powerful tool for making all kinds of personal or industrial expert-system projects. 
Common programming languages have been designed and modified for solving procedural problems but, they can’t easily solve AI (Artificial Intelligence) problems so for this purpose languages like Clips have been created so that they can solve and conduct problems in a very close way like we humans do.

Why building interface?
For developing Clips projects programmers should use either a command based tool or use an IDE that had been created to help programmers but, this environment is not much user friendly for end-users so, by creating an interface for our code we can keep the end-user away from the complications of the system behind the scene and also they can use the program more easily.




Let’s get serious
For creating this interface in this tutorial we are using JNI(Java Native Interface)  which is a Java feature that allows Java to be called from native methods from programming languages such as C/C++ or assembly and also it allows Java to call those methods too.
If you download the binary files of the program you will notice that there exists some .dll files that are actually the assembled files of the language. 
Developers of Clips have also made a JNI project and have published it on their website. On this tutorial we will learn how to use their java libraries (which are the bridge between the C codes in the .dll files and java) to build interface for end-users.

For start you should have a Java IDE such as Eclipse and the Standard Java Development Kit installed on your system.

<img src="https://qjnt2a.bn1301.livefilestore.com/y3muGhQhba6oKs6_1Jzr6ifLqZFuJN989cu17QwEnwKjIuL7HswC3hT57iS0kjJVsGH3Tyc_uAOQO3Lqg1ZPMkyLuAIesI0Ir-FDf05AiuNNjziRSSlArjNs_N5qGarZVLKoJFvNg-rsm7jwStzh8IvA8G2fDrNb9NXH6_5Rh5lPBc?width=312&height=182&cropmode=none" width="312" height="182" />


1.	First you should download all binary files of the Clips programming language and also the JNI libraries from the links bellow.

Binary files:  
https://sourceforge.net/projects/clipsrules/files/latest/download?source=files

JNI projects:
https://sourceforge.net/projects/clipsrules/files/CLIPS/6.30/clips_jni_050.zip/download


2.	Then create an empty Java project and copy all listed files into the project.

<img src="https://qtnt2a.bn1301.livefilestore.com/y3m4dTfbsciRfciqdVAm9F21kaxSesuaxRiUuUjhTNRY6dmdmn99lCn3Vyeqjdoj3nKj2XsWab4vxH5LJT9atW1v2kBTlZLkwWMVZJ4Bk1_sy9qRaq3F38CU11LhRBy9TkED4AD4HogvsCjJ9iWVqDvE4tFFWtOFOmXG-Hax51uDEE?width=302&height=73&cropmode=none" width="302" height="73" />

CLIPSJNI.dll
CLIPSJNI.jar
CLIPSJNI32.dll
CLIPSJNI64.dll



3.	There is a .jar file that should be added to the build path libraries of the Java IDE.

<img src="https://pznt2a.bn1301.livefilestore.com/y3mOX3KIv71tfFQc7O4Enl_Za7ETgna2KBZHzyw-7DQa9m1CiQHiwXCSDDHNK8eID-wRldBLKYqYrF8jBkBa-fol_AhazTnfhV0Ei7F1GYIXK2VkqyMev7xb5esKjgT9ZVJhL4Y_mvM9oTrCfv2GUKj2N0s6aSWPFbGYa5Q8zTEyUc?width=403&height=216&cropmode=none" width="403" height="216" />


4.	Finally a .clp file containing all rules and templates of your project should be created and be copied to the root folder of your java project. For instance I Have created a very simple rule in Clips Programming with two templates for holding my facts and I have put it in the root folder of the Java project for easier access to the file.

(deftemplate student
	(slot study)
)
(deftemplate result
	(slot value )
)

(defrule student-rule 
	 (student (study ?s))
  =>
  	(if (eq ?s Yes) 
  		then
  		(bind ?x "Well Done! you will pass all the tests.")
 		(printout t ?x crlf)
 		(assert (result (value ?x)))
 	else 
 		(bind ?x "You should probably study more in case you want to pass your tests.")
 		(printout t ?x crlf) 
 		(assert (result (value ?x)))
 	)
)


Here is a very simple UI have been created with only a single combo box for our fact and a button to execute the inference engine and a text area to show the results.

<img src="https://pdnt2a.bn1301.livefilestore.com/y3mZ6G06UWiwhfoG6HOyKFgL34iDCipOFXsVns7Zq_UZxL6oqsOB4wumoihYskU5vU5uOrME35k8_yG_c9LlU8IwC_7HIiXHbp47_YD60rqckFiCSoXTfN1TeviGNeQL3iV_H8dSh69PFXX-I50yN3Dfg5wqwRdKzZ3GqIhXGoKjIc?width=359&height=229&cropmode=none" width="359" height="229" />



First we should define an instance of clips jni library:
private static net.sf.clipsrules.jni.Environment clips;

Then the defined instance should be instantiated and be loaded with data in the .clp file that we have coded in Clips Language.
clips = new Environment();
clips.load("a.clp");	
clips.reset();


For inserting facts to the system:
clips.assertString("(student (study No))"); 
clips.assertString("(student (study Yes))"); 

Finally with run method we will be able to start the inference engine.
clips.run();

For getting back the conducted results bellow code should be used:
MultifieldValue mv = (MultifieldValue) clips.eval("(find-all-facts ((?r result)) TRUE)");
FactAddressValue fact = (FactAddressValue) mv.multifieldValue().get(0);
PrimitiveValue pv = fact.getFactSlot("value");
String ss = pv.toString();
textArea.setText(ss);

The above code searches for all the facts with name of (result) with the help of find-all-facts method which is a known Clips Programming language function and get them for further processing steps. Later on the returned result had been printed into a text area.




The End

Download Link: https://assignmentchef.com/product/solved-acs-202-computer-science-ii-project-xtra
<br>



<strong> </strong>







<strong>Objectives:  </strong>The main objectives of this project are to review and strengthen your ability to create and use dynamic memory wrapped in classes (and also to ameliorate your Grade!)




<strong>Description: </strong>

For this project you will create your own <strong>SmartPtr (Smart Pointer) </strong>class. A Smart Pointer serves the purpose of wrapping a set of useful behaviors around a common Raw Pointer, such as:

<ol>

 <li>Automatically handle allocation of Dynamic Memory if necessary, when a SmartPointer object is created. ii. Automatically handle deallocation of Dynamic Memory if appropriate, when a SmartPointer object lifetime ends.</li>

</ol>

<ul>

 <li>Provide access to the Dynamic Memory it encapsulates (via the actual Raw Pointer) using the same notation (the same operators) as a Raw Pointer, so that it is exactly as easy to use.</li>

</ul>

<ol>

 <li>Automatically handle cases such as a) when a Smart Pointer is used to point to the data already allocated by another SmartPointer, and avoid re-allocation, or b) when a</li>

</ol>

SmartPointer’s lifetime ends but there also exists another SmartPointer pointing to the same data, and avoid deallocating early (understand when the last SmartPointer corresponding to that memory is destroyed, and only then delete the data).

The following header file extract gives the required specifications for the class:




<strong>//Necessary preprocessor #define(s) </strong>

<strong>… </strong>

<strong>//Necessary include(s) </strong>

<strong>… </strong>

<h1>//Class specification class SmartPtr{</h1>

<strong> </strong><strong>  </strong><strong><em>public:</em></strong>

<strong>    </strong><strong>SmartPtr</strong><strong>( );                                            </strong><strong>//(1)</strong>

<strong>    </strong><strong>SmartPtr</strong><strong>( </strong><strong>DataType *</strong><strong> data );                             </strong><strong>//(2)</strong>

<h1>    SmartPtr( const SmartPtr&amp; other );                        //(3)</h1>

<strong>     </strong>

<strong>    ~SmartPtr</strong><strong>( );                                           </strong><strong>//(4)</strong>

<strong>                 </strong>

<strong>    </strong><strong>SmartPtr&amp;</strong> <strong>operator=</strong><strong>( const SmartPtr&amp; rhs );                </strong><strong>//(5)</strong>

<strong> </strong>

<h1>    DataType&amp; operator*( );                                  //(6)     DataType* operator-&gt;( );                               //(7)</h1>

<strong>                 </strong><strong>  </strong><strong><em>private: </em></strong>

<strong>    </strong><strong>size_t * </strong><strong>m_refcount;                               </strong><strong>//(8)</strong><strong>     </strong><strong>DataType *</strong><strong> m_ptr;                    </strong><strong>                   //(9)</strong>

<strong>                 </strong>

<strong>}; </strong>

<strong>… </strong>




Specifications explained:




You will notice that the Smart Pointer encapsulates a raw pointer <strong>m_ptr</strong> of type <strong>DataType</strong> <strong>(9)</strong>.  (This means that the Smart Pointer works with dynamically allocated DataType objects, but since DataType is a class that you can define yourselves, it is still flexible and modular enough given what C++ practices we know so far).

<strong>DataType</strong> is a Class which is given to you (Header .h &amp; Implementation .cpp files both), and it is simple enough to be considered self-explanatory.




The <strong>SmartPtr </strong>Class will contain the following<strong> private </strong>data members:

<ul>

 <li><strong>(9) m_ptr,</strong> a DataType Pointer, <u>pointing</u> to the <u>Dynamically Allocated</u> These is the Dynamic Memory encapsulated by the SmartPtr class, i.e. the memory that needs to be: a) allocated by the class’ own methods when appropriate,

  <ol>

   <li>deallocated by the class’ own methods when appropriate,</li>

   <li>addressable and accessible via the class’ own methods.</li>

  </ol></li>

 <li><strong>(8) m_refcount</strong>, a size_t Pointer, pointing to a dynamically allocated positive integer (size_t) variable. It is a reference-counting helper variable, keeping track of how many SmartPtr objects refer to the same Dynamic Memory behind m_ptr.</li>

</ul>

<u>The <em>value</em> (0,1,2,…) <em>pointed-to</em> by m_refcount denotes how many SmartPointer objects are currently pointing to the same Dynamically Allocated memory as the one pointed by m_ptr</u>.

But m_refcount is not a size_t but a Pointer to a size_t.

<u>This is because it needs to be a <em>shared value</em> between different SmartPtr objects, so that when one SmartPtr object updates it, all others can see the change.</u> E.g. when one SmartPtr object gets destroyed, it should update the <em>value pointed-to</em> by m_refcount by decrementing it, to denote that there is one less SmartPtr object alive that points to the Dynamically Memory pointed to by m_ptr.

and will have the following<strong> public </strong>member functions:

<ul>

 <li><strong>(1) Default Constructor</strong> – will:

  <ol>

   <li>Dynamically Allocate a new DataType object and keep track of its address via m_ptr.</li>

   <li>Dynamically allocate a new size_t variable and set the <em>value it points-to</em> to 1 to denote that there exists one SmartPtr object pointing the newly allocated Dynamic Memory behind m_ptr.</li>

  </ol></li>

</ul>

<em>Remember</em>: This will be assigned to m_refcount and will be <em>shared</em> among any other future SmartPtr objects that will be used to point to the same Dynamic Memory as the one behind m_ptr, so that they all know when it is time to deallocate that memory. c) Right before it returns, it should print out:

“SmartPtr Default Constructor for new allocation, RefCount=&lt;refcount&gt;” where   &lt;refcount&gt; the actual <em>value</em> <em>pointed-to</em> by m_refcount.

<ul>

 <li><strong>(2) Parametrized Constructor </strong>– will take a Pointer to DataType as a parameter. This means that this Constructor takes in already pre-allocated data, and wraps itself around that raw pointer. It will:

  <ol>

   <li>Not perform any Dynamic Allocation since the data Pointer should correspond to pre-allocated data, therefore it will use m_ptr to keep track of that data directly.</li>

   <li>Dynamically allocate a new size_t variable and keep track of it via m_refcount. Depending on whether the data Pointer passed is NULL or not, the <em>value</em> <em>pointed-to</em> by m_refcount should be set to 0 or 1 to denote that the SmartPtr object does not correspond to valid memory, or that there exists one SmartPtr object pointing the Dynamic Memory behind m_ptr.</li>

   <li>Right before it returns, it should print out:</li>

  </ol></li>

</ul>

“SmartPtr Parametrized Constructor from data pointer, RefCount=&lt;refcount&gt;” where   &lt;refcount&gt; the actual <em>value</em> <em>pointed-to</em> by m_refcount.

<ul>

 <li><strong>(3) Copy Constructor</strong> – will take another SmartPtr object as a parameter. This means that this Constructor has access to the pre-allocated data of the other object, and the preallocated reference-counting variable too (which will already have a <em>point-to value</em>).</li>

</ul>

<ol>

 <li>Not perform any Dynamic Allocation since the other object’s m_ptr should correspond to pre-allocated data, therefore it will use m_ptr to keep track of that data directly.</li>

 <li>Bind its m_refcount to the same <em>shared</em> reference-counting variable as the other object’s m_refcount when appropriate.</li>

</ol>

<em>Note</em>: Depending on whether the other object’s m_ptr is NULL or not, the m_refcount of the newly instantiated SmartPtr should either be newly allocated (and the <em>value it points-to</em> should be initialized to 0), or it should be bound to the other object’s m_refcount and the <em>value it points-to</em> should be incremented (++), to denote that there now exists one additional SmartPtr object pointing the Dynamic Memory behind m_ptr.

<em>Hint</em>: Generally speaking in this implementation, SmartPtr objects corresponding to the same Dynamic Memory should also be bound to the same *m_refcount object. SmartPtr objects that correspond to no valid Dynamic Memory however (NULL), should each have their own *m_refcount object.

<ol>

 <li>Right before it returns, it should print out:</li>

</ol>

“SmartPtr Copy Constructor, RefCount=&lt;refcount&gt;” where &lt;refcount&gt; the actual   <em>value pointed-to</em> by m_refcount.

<ul>

 <li><strong>(4) Destructor </strong>– will:

  <ol>

   <li>Decrement the <em>value</em> <em>pointed-to</em> by m_refcount to denote that one less SmartPtr object is now pointing to the Dynamic Memory behind m_ptr.</li>

   <li>Examine whether the calling SmartPtr objectd (the one whose lifetime is just now expiring, so its Destructor is getting called) is the <em>last</em> one referencing the Dynamic Memory behind m_ptr. To do that, it will have to examine the <em>value pointed-to</em> by m_refcount (after it has been decremented).</li>

   <li>If it is the last one, it should dellocate the Dynamic Memory both behind m_ptr, and the shared variable m_refcount.</li>

   <li>Right <u>before any deallocation happens</u>, it should print out:</li>

  </ol></li>

</ul>

“SmartPtr Destrcutor, RefCount=&lt;refcount&gt;” where &lt;refcount&gt; the actual   <em>value pointed-to</em> by m_refcount.

<ul>

 <li><strong>(5) operator= </strong>will perform assignment from a SmartPtr object. This means that it will:

  <ol>

   <li>First take care of releasing its handle on its own Dynamic Memory. <em>Note</em>: This does not necessarily mean to directly deallocate it, there might be other SmartPtr objects referencing the same Dynamic Memory! Review the description of the Destructor to understand how releasing with respect for other SmartPtr objcets will need to work.</li>

   <li>Then switch to referencing the same values as the other SmartPtr object. This means that both m_ptr and m_refcount will need to be repointed there, and any additional considerations (such as mutating the <em>value pointed-by</em> m_refcount, and how to handle a case where the other object holds a NULL pointed for its Dynamic Memory, etc) should be handled as per the Copy Constructor.</li>

   <li>Right before it returns, it should print out:</li>

  </ol></li>

</ul>

“SmartPtr Copy Assignment, RefCount=&lt;refcount&gt;” where &lt;refcount&gt; the actual   <em>value pointed-to</em> by m_refcount.

<ul>

 <li><strong>(6) operator* </strong>will act in the same way that it is used on Raw Pointers, i.e. it will <u>Dereference</u> <u>the Dynamic Memory Object</u> that is encapsulated within the SmartPtr:</li>

</ul>

When you have a Raw Pointer, dereferencing returns a Reference-to the underlying object:

<strong>DataType *</strong><strong> data_pt = </strong><strong>new</strong> <strong>DataType</strong><strong>;  </strong><strong>//data_pt is a raw pointer </strong><strong>DataType &amp;</strong><strong> data_ref = </strong><strong>*</strong><strong>( data_pt ); </strong><strong>//operator* on a raw pointer</strong>

In the same principle, operator* will act as a Smart Pointer Dereference:

<strong>SmartPtr </strong><strong>data_pt;                   </strong><strong>//data_pt is now a smart pointer</strong> <strong>DataType &amp;</strong><strong> data_ref = </strong><strong>*</strong><strong>( data_pt ); </strong><strong>//operator* on a smart pointer</strong> and again have the same result (the goal is to maintain the same notation semantics so that the user of a SmartPtr class has near-zero things to learn in order to use it).

<em>Hint:</em> Given the SmartPtr class declaration that you already know, where you are also given the return type, it should be straightforward how to implement this method.

<ul>

 <li><strong>(7) operator-&gt; </strong>will act in the same way that it is used on Raw Pointers, i.e. it will <u>Allow Object Member Access via the Dynamic Memory Pointer</u> that is encapsulated within the SmartPtr:</li>

</ul>

When you have a Raw Pointer, member-access is performed like:

<strong>DataType *</strong><strong> data_pt = </strong><strong>new</strong> <strong>DataType</strong><strong>;  </strong><strong>//data_pt is a raw pointer </strong><strong>int</strong><strong> intVar = data_pt</strong><strong>-&gt;</strong><strong>GetIntVar</strong><strong>();  </strong><strong>//operator-&gt; on a raw pointer</strong>

In the same principle, operator-&gt; will act as a Member Access via Smart Pointer:

<strong>SmartPtr </strong><strong>data_pt;                   </strong><strong>//data_pt is now a smart pointer</strong> <strong>int</strong><strong> intVar = data_pt-&gt;</strong><strong>GetIntVar</strong><strong>();  </strong><strong>//operator-&gt; on a smart pointer</strong> and again have the same result (the goal is to maintain the same notation semantics so that the user of a SmartPtr class has near-zero things to learn in order to use it).

<em>Hint:</em> Given the SmartPtr class declaration that you already know, where you are also given the return type, it should be straightforward how to implement this method.




The SmartPtr.h header file should be as per the specifications. The SmartPtr.cpp source file you create will hold the required implementations. You should also create a source file projX.cpp which will be a test driver for your class.




The test driver has to demonstrate that your SmartPtr class works as specified:

Ø You should use all the meaningful test cases you can identify to demonstrate the use of all the class methods. The following example is considered as a starting point:




<strong>cout</strong><strong> &lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Default ctor”</strong><strong> &lt;&lt; endl;   </strong><strong>SmartPtr</strong><strong> sp1;  </strong><strong>// Default-ctor</strong><strong> sp1</strong><strong>-&gt;</strong><strong>SetIntVal</strong><strong>(1); sp1</strong><strong>-&gt;</strong><strong>SetDoubleVal</strong><strong>(0.25); </strong>

<h2>cout &lt;&lt; “Dereference Smart Pointer 1: ” &lt;&lt; *sp1 &lt;&lt; endl;</h2>

<strong> </strong>

<strong>cout </strong><strong>&lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Copy ctor”</strong><strong> &lt;&lt; endl; </strong><strong>SmartPtr</strong><strong> sp2 = sp1;  </strong><strong>// Copy-initalization (Copy-ctor)</strong><strong> sp2-&gt;</strong><strong>SetIntVal</strong><strong>(2); sp2-&gt;</strong><strong>SetDoubleVal</strong><strong>(0.5); </strong><strong>cout</strong><strong> &lt;&lt; </strong><strong>“Dereference Smart Pointer 1: “</strong><strong> &lt;&lt; </strong><strong>*</strong><strong>sp1 &lt;&lt; endl; </strong><strong>cout</strong><strong> &lt;&lt; </strong><strong>“Dereference Smart Pointer 2: “</strong><strong> &lt;&lt; </strong><strong>*</strong><strong>sp2 &lt;&lt; endl; </strong><strong>cout</strong><strong> &lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Assignment operator”</strong><strong> &lt;&lt; endl; </strong><strong>SmartPtr</strong><strong> sp3; sp3 = sp1;  </strong><strong>// Assignment operator</strong><strong> sp3</strong><strong>-&gt;</strong><strong>SetIntVal</strong><strong>(4); sp3</strong><strong>-&gt;</strong><strong>SetDoubleVal</strong><strong>(0.0); </strong><strong>cout </strong><strong>&lt;&lt; </strong><strong>“Dereference Smart Pointer 1: ” </strong><strong>&lt;&lt; </strong><strong>*</strong><strong>sp1 &lt;&lt; endl; </strong><strong>cout</strong><strong> &lt;&lt; </strong><strong>“Dereference Smart Pointer 2: “</strong><strong> &lt;&lt; </strong><strong>*</strong><strong>sp2 &lt;&lt; endl; </strong><strong>cout </strong><strong>&lt;&lt; </strong><strong>“Dereference Smart Pointer 3: “</strong><strong> &lt;&lt; </strong><strong>*</strong><strong>sp3 &lt;&lt; endl; </strong>

<strong> </strong><strong>cout </strong><strong>&lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Parametrized ctor with NULLdata” </strong><strong>&lt;&lt; endl; </strong>

<strong>SmartPtr</strong><strong> spNull( </strong><strong>NULL</strong><strong> );  </strong><strong>// NULL-data initialization</strong>

<strong> </strong><strong>cout </strong><strong>&lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Copy ctor with NULLdata SmartPtr” </strong><strong>&lt;&lt; endl; </strong><strong>SmartPtr </strong><strong>spNull_cpy( spNull );  </strong><strong>// NULL-data copy constructor </strong>

<strong> </strong><strong>cout </strong><strong>&lt;&lt; endl &lt;&lt; </strong><strong>“Testing SmartPtr Assignment with NULLdata SmartPtr” </strong><strong>&lt;&lt; endl; </strong><strong>SmartPtr</strong><strong> spNull_assign; spNull_assign = spNull;  </strong><strong>// NULL-data assign</strong>

<strong> </strong><strong>cout</strong><strong> &lt;&lt; endl &lt;&lt;  </strong>

<strong>“End-of-Scope, Destructors called in reverse order of SmartPtr creation
 (spNull_assign, spNull_cpy, spNull, sp3, sp2, sp1): “</strong><strong> &lt;&lt; endl;</strong>

<strong> </strong>

<strong>Do not forget to explain what is happening in the output of your projX.cpp test driver in your Documentation file. </strong>

<strong> </strong>

<strong>This Project is an EXTRA! It can help you ameliorate your Grade, but you can also choose to skip it altogether… </strong>




<strong>The completed project should have the following properties: </strong> Ø Written, compiled and tested using Linux.

<ul>

 <li>It must compile successfully on the department machines using Makefile(s), which will be invoking the g++ compiler. Instructions how to remotely connect to department machines are included in the Projects folder in WebCampus.</li>

 <li>The code must be commented and indented properly.</li>

</ul>

Header comments are required on all files and recommended for the rest of the program. Descriptions of functions commented properly.

<ul>

 <li>A one page (minimum) typed sheet documenting your code. This should include the overall purpose of the program, your design, problems (if any), and any changes you would make given more time.</li>

</ul>

<strong> </strong>

<strong>Turn in:</strong> Compressed Header &amp; Source files, Makefile(s), and project documentation.




<strong>Submission Instructions: </strong>

<ul>

 <li>You will submit your work via WebCampus</li>

 <li>The code file projX.cpp is already provided and implements a test driver.</li>

 <li>If you have header file, name it projX.h</li>

 <li>If you have class header and source files, name them as the respective class (SmartPtr.h SmartPtr.cpp) This source code structure is now mandatory.</li>

 <li>Compress your:

  <ol>

   <li>Source code</li>

   <li>Makefile(s)</li>

   <li>Documentation Do not include executable  Ø Name the compressed folder:</li>

  </ol></li>

</ul>

PA#_Lastname_Firstname.zip

([PA] stands for [ProjectAssignment], [#] is the Project number)  Ex: PAX_Smith_John.zip







<strong>Late Submission:</strong>

A project submission is “late” if any of the submitted files are time-stamped after the due date and time. Projects will be accepted up to 24 hours late, with 20% penalty.


















!SLIDE
# boostrap.php
	@@@ php
	require dirname(__FILE__) .
		'/../index.php';


!SLIDE
# bootstrap.php

	@@@ php
	require __DIR__ . '/../index.php';


!SLIDE
# system/core/CodeIgniter.php

	@@@ php
	...
	if (ENVIRONMENT === 'testing') {
		$GLOBALS += get_defined_vars();
	}
	...


!SLIDE
# Testing Models and Helpers


!SLIDE

		@@@ php
    class ThingTest extends
        PHPUnit_Framework_TestCase {

        public function setUp() {
            $this->ci = &get_instance();
            $this->ci->load->model('thing');
        }

        public function testMethod_Does() {
            $v = $this->ci->thing->doSomething();
            $this->assertEquals('123', $v);
        }
    }


!SLIDE
# Testing Libraries
## [github.com/katzgrau/getsparks.org](https://github.com/katzgrau/getsparks.org)


!SLIDE
# Stubbing and Mocking
## Its place in your tests
* Stubbing vs. Mocking


!SLIDE
	@@@ php
	$stub = $this->getMock('Thing');
	$stub->expects($this->once())
			->method('someMethod')
			->will($this->returnValue('hello'));


!SLIDE
# Testing Controllers and Views


!SLIDE
# Controllers are classes


!SLIDE
# Output_Test_Case


!SLIDE
	@@@ php
	...
	public function testOut() {
		$this->expectOutputString('hello');
		$controller = new Welcome();
		$controller->index();
	}
	...


!SLIDE
	@@@php
	...
	public function testOut() {
		$this->expectOutputRegex('/hello/');
		$controller = new Welcome();
		$controller->index();
	}
	...


!SLIDE
# Handling External Requests
* Mocking
* `nice_http` or equivelant


!SLIDE
# nice_http
## [github.com/seejohnrun/nice_http](https://github.com/seejohnrun/nice_http)


!SLIDE
	@@@ php
	NiceHTTP::disallowExternalConnections();

	NiceHTTP::register(function($request) {
		if ($request->hasPath('/'))
		{
			return NiceHTTP\BasicResponse(
				200,
				'hello world'
			);
		}
	});

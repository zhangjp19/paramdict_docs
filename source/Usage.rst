Usage
=====

Class Definition
-----------------

Here is the smallest paramdict class definition:

.. code:: python

    from paramdict import *

    class pdTest(baseParamDict):

        def __init__(self, id:str='', source=None):
            super().__init__(id, source)
        def _setcls(self):
            return 'pdtest'
        def _pdinit(self):
            pass
        def _check(self):
            return super()._check()

        # Parameter definitions ...

A paramdict class must inherit from :py:class:`paramdict.paramDictHandler.baseParamDict` and implement the following methods:

* ``__init__``: Constructor. It should at least call the constructor of the base class.
* ``_setcls``: Return the class name of the paramdict. This is a fundamental identifier for the paramdict.
* ``_pdinit``: When a paramdict instance is created without a source, this method is called to initialize the parameters. User can set default values for parameters here.
* ``_check``: When a paramdict instance is created with a source, this method is called to check the validity of the parameters. User can implement specific validation logic here.

Parameter Definition
--------------------

Parameters are defined by setting decorated class methods of the paramdict class. :py:mod:`paramdict.paramDictProperties` provides various types of built-in parameter property decorators. Applying a property decorator to a class attribute will automatically make it a parameter of the paramdict. Most of them do runtime type checking. This mechanism relies on the return type annotation of the decorated method.

Built-in Decorators
^^^^^^^^^^^^^^^^^^^

* :py:class:`paramdict.paramDictProperties.pSimple`

.. code:: python

    @pSimple
    def param_simple(self) -> int: ...

A simple parameter. When setting, the value is converted to the annotated type and stored. Type should support the conversion format :code:`type(value)`. When getting, the value is retrieved directly from the storage.

* :py:class:`paramdict.paramDictProperties.pSimpleOpt`

.. code:: python

    @pSimpleOpt
    def param_simple_opt(self) -> int: ...

An optional simple parameter. Similar to :py:class:`paramdict.paramDictProperties.pSimple`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_simple_opt`.

* :py:class:`paramdict.paramDictProperties.pStrict`

.. code:: python

    @pStrict
    def param_strict(self) -> int: ...

A parameter with strict type checking. When setting, the value is checked to be of the annotated type. When getting, the value is retrieved directly from the storage.

* :py:class:`paramdict.paramDictProperties.pStrictOpt`

.. code:: python

    @pStrictOpt
    def param_strict_opt(self) -> int: ...

An optional parameter with strict type checking. Similar to :py:class:`paramdict.paramDictProperties.pStrict`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_strict_opt`.

* :py:class:`paramdict.paramDictProperties.pList`

.. code:: python

    @pList
    def param_list(self) -> list[int]: ...

A list of parameters with unified type. When setting, the value is checked to be a list, and the type of each element is checked to be the annotated type. When getting, the value is retrieved directly from the storage.

.. note:: If type checking for elements is unnecessary, user may prefer :py:class:`paramdict.paramDictProperties.pSimple` with a list type annotation:

.. code:: python

    @pSimple
    def param_list_simple(self) -> list: ...

* :py:class:`paramdict.paramDictProperties.pListOpt`

.. code:: python

    @pListOpt
    def param_list_opt(self) -> list[int]: ...

An optional list of parameters with unified type. Similar to :py:class:`paramdict.paramDictProperties.pList`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_list_opt`.

* :py:class:`paramdict.paramDictProperties.pEnm`

.. code:: python

    from enum import Enum

    class Color(Enum):
        RED = 1
        GREEN = 2
        BLUE = 3

    @pEnm
    def param_enm(self) -> Color: ...

A parameter with enumeration type. When setting, the value is checked to be a member of the annotated Enum type and is stored by its value. When getting, the value is retrieved from the storage and returned as the corresponding Enum member.

* :py:class:`paramdict.paramDictProperties.pEnmOpt`

.. code:: python

    @pEnmOpt
    def param_enm_opt(self) -> Color: ...

An optional parameter with enumeration type. Similar to :py:class:`paramdict.paramDictProperties.pEnm`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_enm_opt`.

* :py:class:`paramdict.paramDictProperties.pEnmList`

.. code:: python

    @pEnmList
    def param_enm_list(self) -> list[Color]: ...

A list of parameters with enumeration type. When setting, the value is checked to be a list, and each element is checked to be a member of the annotated Enum type and stored by its value. When getting, the value is retrieved from the storage and returned as a list of corresponding Enum members.

* :py:class:`paramdict.paramDictProperties.pEnmListOpt`

.. code:: python

    @pEnmListOpt
    def param_enm_list_opt(self) -> list[Color]: ...

An optional list of parameters with enumeration type. Similar to :py:class:`paramdict.paramDictProperties.pEnmList`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_enm_list_opt`.

* :py:class:`paramdict.paramDictProperties.pPd`

.. code:: python

    @pPd
    def param_pd(self) -> pdTest2: ...

A parameter with paramdict type. When setting, the value is checked to be an instance of the annotated paramdict type and stored by its dictionary representation. When getting, the value is retrieved from the storage and returned as an instance of the annotated paramdict type.

* :py:class:`paramdict.paramDictProperties.pPdOpt`

.. code:: python

    @pPdOpt
    def param_pd_opt(self) -> pdTest2: ...

An optional parameter with paramdict type. Similar to :py:class:`paramdict.paramDictProperties.pPd`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_pd_opt`.

* :py:class:`paramdict.paramDictProperties.pPdList`

.. code:: python

    @pPdList
    def param_pd_list(self) -> list[pdTest2]: ...

A list of parameters with paramdict type. When setting, the value is checked to be a list, and each element is checked to be an instance of the annotated paramdict type and stored by its dictionary representation. When getting, the value is retrieved from the storage and returned as a list of corresponding paramdict instances.

.. note:: If type checking for elements is unnecessary, user may prefer :py:class:`paramdict.paramDictProperties.pSimple` with a list type annotation:

.. code:: python

    @pSimple
    def param_pd_list_simple(self) -> list: ...

* :py:class:`paramdict.paramDictProperties.pPdListOpt`

.. code:: python

    @pPdListOpt
    def param_pd_list_opt(self) -> list[pdTest2]: ...

An optional list of parameters with paramdict type. Similar to :py:class:`paramdict.paramDictProperties.pPdList`, but when getting, if the value is not set, returns :py:attr:`paramdict.paramDictProperties.UNSET`. Can be deleted using :code:`del pd_instance.param_pd_list_opt`.

Using Python Property Decorators
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In some cases, user may want to define parameters with custom getter and setter logic. Using Python's built-in property decorator is an option. Setter should store the value in the storage dictionary ``_cont``, and getter should retrieve the value from ``_cont``. User can also implement custom deleter to support deletion of the parameter.

For example, we want to constrain a parameter ``x`` to be a positive integer:

.. code:: python

    @property
    def x(self) -> int:
        return self._cont['x']
    @x.setter
    def x(self, value):
        value = int(value)
        if value <= 0:
            raise ValueError('x should be a positive integer')
        self._cont['x'] = value

Define Extra Decorators
^^^^^^^^^^^^^^^^^^^^^^^

When similar constraints or logic are needed for multiple parameters, simply using python property decorators may cause code repetition. Defining extra decorators to implement custom parameter types is a better option. The decorator can inherit from built-in decorators in :py:class:`paramdict.paramDictProperties.baseParamProperty`, or solely created by user. 

For example, we can define a decorator for positive integer parameters:

.. code:: python

    class pPosInt(pSimple):
    
        def __set__(self, instance, value):
            value = int(value)
            if value <= 0:
                raise ValueError('Value should be a positive integer')
            super().__set__(instance, value)

This decorator is easy to use:

.. code:: python

    @pPosInt
    def x(self) -> int: ...

    @pPosInt
    def y(self) -> int: ...


Set Default Values for Parameters
---------------------------------

When a paramdict instance is created without a source, the method ``_pdinit`` is called to initialize the parameters. User can set default values for parameters in this method. For example:

.. code:: python

    class pdTest(baseParamDict):

        def __init__(self, id:str='', source=None):
            super().__init__(id, source)
        def _setcls(self):
            return 'pdtest'
        def _pdinit(self):
            self.param_simple = 0
            self.param_list = [1, 2, 3]
        def _check(self):
            return super()._check()
        
        @pSimple
        def param_simple(self) -> int: ...

        @pList
        def param_list(self) -> list[int]: ...

In this example, when a pdTest instance is created without a source, the parameter ``param_simple`` will be initialized to 0, and the parameter ``param_list`` will be initialized to [1, 2, 3].

Serialization and Deserialization
---------------------------------

All information of a paramdict instance is stored in the storage dictionary, which can be accessed from a property :py:attr:`paramdict.paramDictHandler.baseParamDict.content`. A popular serialization format is JSON, which can be easily achieved by using the built-in :py:mod:`json` module:

.. code:: python

    import json

    pd_instance = pdTest()
    # Set parameters of pd_instance ...

    with open('pd_instance.json', 'w') as f:
        json.dump(pd_instance.content, f)

Similarly, deserialization can be done by loading the JSON file and passing the dictionary to the constructor of the paramdict class:

.. code:: python

    with open('pd_instance.json', 'r') as f:
        content = json.load(f)
    pd_instance = pdTest(source=content)

.. note:: If the 'class' field in the content dictionary does not match the class name returned by the ``_setcls`` method of the paramdict class, a TypeError will be raised. This is a built-in rule and convention to avoid accidentally deserializing mismatched content, because the validity of the content cannot be guaranteed in that case and, in a real application, may cause complex bugs.

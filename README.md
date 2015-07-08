Django IPware
====================

**A Django application to retrieve user's IP address**

[![status-image]][status-link]
[![version-image]][version-link]
[![coverage-image]][coverage-link]
[![download-image]][download-link]


Overview
====================

**Best attempt** to get user's (client's) real ip-address while keeping it **DRY**.

How to install
====================

    1. easy_install django-ipware
    2. pip install django-ipware
    3. git clone http://github.com/un33k/django-ipware
        a. cd django-ipware
        b. run python setup.py
    4. wget https://github.com/un33k/django-ipware/zipball/master
        a. unzip the downloaded file
        b. cd into django-ipware-* directory
        c. run python setup.py


How to use
====================

   ```python
    # If your web server is publicly accessible on the Internet
    # =========================================================
    # To get the `real`, `public` IP address of the client.
    # Where:
    #    `Real IP` = Client's IP and not that of any `in-between` proxies.
    #    `Public IP` = Any IP address that is route-able on the Internet

    from ipware.ip import get_real_ip
    ip = get_real_ip(request)
    if ip is not None:
       # we have a real, public ip address for user
    else:
       # we don't have a real, public ip address for user


    # If your web server is NOT publicly accessible on the Internet
    # =============================================================
    # To get the `best matched` IP address of the client.
    # Where:
    #    `Best Matched IP` = The first matched public IP if found, else the first matched non-public IP.

    from ipware.ip import get_ip
    ip = get_ip(request)
    if ip is not None:
       # we have an ip address for user
    else:
       # we don't have an ip address for user

    # By default the left most address in the `HTTP_X_FORWARDED_FOR` is returned. However, depending on your
    # preference and needs, you can change this behavior by passing the `right_most_proxy=True` to the API.
    # Please note that not all proxies are equal. So left to right or right to left is not a rule that all
    # proxy servers follow.
    from ipware.ip import get_ip
    ip = get_ip(request, right_most_proxy=True)
    # OR
    ip = get_real_ip(request, right_most_proxy=True)
   ```

Advanced users:
====================

   ```python
    # you can provide your own meta precedence order by
    # including IPWARE_META_PRECEDENCE_ORDER in your
    # settings.py. The check is done from top to bottom
    IPWARE_META_PRECEDENCE_LIST = (
        'HTTP_X_FORWARDED_FOR', # client, proxy1, proxy2
        'HTTP_CLIENT_IP',
        'HTTP_X_REAL_IP',
        'HTTP_X_FORWARDED',
        'HTTP_X_CLUSTER_CLIENT_IP',
        'HTTP_FORWARDED_FOR',
        'HTTP_FORWARDED',
        'HTTP_VIA',
        'REMOTE_ADDR',
    )

    # you can provide your own private IP prefixes by
    # including IPWARE_PRIVATE_IP_PREFIX in your setting.py
    # IPs that start with items listed below are ignored
    # and are not considered a `real` IP address
    'IPWARE_PRIVATE_IP_PREFIX', (
        '0.',  # externally non-routable
        '10.',  # class A private block
        '169.254.',  # link-local block
        '172.16.', '172.17.', '172.18.', '172.19.',
        '172.20.', '172.21.', '172.22.', '172.23.',
        '172.24.', '172.25.', '172.26.', '172.27.',
        '172.28.', '172.29.', '172.30.', '172.31.',  # class B private blocks
        '192.0.2.',  # reserved for documentation and example code
        '192.168.',  # class C private block
        '255.255.255.',  # IPv4 broadcast address
    ) + (
        '2001:db8:',  # reserved for documentation and example code
        'fc00:',  # IPv6 private block
        'fe80:',  # link-local unicast
        'ff00:',  # IPv6 multicast
    )
   ```

Running the tests
====================

To run the tests against the current environment:

    python manage.py test


License
====================

Released under a ([BSD](LICENSE.md)) license.


Version
====================
X.Y.Z Version

    `MAJOR` version -- when you make incompatible API changes,
    `MINOR` version -- when you add functionality in a backwards-compatible manner, and
    `PATCH` version -- when you make backwards-compatible bug fixes.

[status-image]: https://secure.travis-ci.org/un33k/django-ipware.png?branch=master
[status-link]: http://travis-ci.org/un33k/django-ipware?branch=master

[version-image]: https://img.shields.io/pypi/v/django-ipware.svg
[version-link]: https://pypi.python.org/pypi/django-ipware

[coverage-image]: https://coveralls.io/repos/un33k/django-ipware/badge.svg
[coverage-link]: https://coveralls.io/r/un33k/django-ipware

[download-image]: https://img.shields.io/pypi/dm/django-ipware.svg
[download-link]: https://pypi.python.org/pypi/django-ipware

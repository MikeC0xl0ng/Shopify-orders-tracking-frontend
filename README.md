# Shopify Orders

In order to access `order` object it's necessary to check if client obj exists (if the client logged in), we can do it in the following way:
```
{% if customer %}
    // check the orders
{% else %}
    // redirect to login page
{% endif %}
```

To obtain orders of a client in Liquid template we must  use `customer.orders`:  in this case we will get all the orders of that specific client. With a simple `{% for order in customer.orders %}` we can loop through all the orders of the client. 

## Order

`order` object has a lot of attributes, the most useful are the following ones:

### Order info
*  order.name

Returns the name of the order in the format set in the `Standards and formats` section of the General settings of your Shopify admin (by Default is the order number preceded by `#`, e.g. `#1003`)
* order.order_number

Returns the integer representation of the order name (if we considered the case above, it would be `1003`)
* order.email

Returns the email address associated with an order (if it exists)
* order.phone

Returns the phone number associated with an order
* order.fulfillment_status

Returns the fulfillment status of an order, can have the following forms:
    - Fulfilled
    - Unfulfilled
    - Partially fulfilled
    - Scheduled
    - On hold
* order.fulfillment_status_label

Returns the translated output of an order's fulfillment_status.
* order.created_at 

Returns the timestamp of when an order was created. Use the [date filter](https://shopify.dev/api/liquid/filters/additional-filters#date) to format the timestamp.
* order.line_items

Returns an array of line items for the order
* order.customer_url

Returns a unique URL that the customer can use to access the order.
* order.order_status_url

Returns the unique URL for the [order status page](https://help.shopify.com/en/manual/orders/status-tracking?shpxid=3ea7fd6d-7C31-4585-CE09-4C46B99E8678) of the order.
* order.total_price
Returns the total price of an order

### Order Customer
* order.customer

Returns the customer obj associated with an order
* order.customer_url

Returns a unique URL that the customer can use to access the order

## Tracking an order

In Shopify an order can be composed from more physical items, so it is possible that a merchant keeps them in different warehouses. For this case Shopify has a tracking feature to track every item of an order. 

In case we wanted to create a page for tracking orders, we would need to display every item and show the tracking number (with or without tracking URL) for each of them. 

Basicaly thre structure would be:
Order info: name, id, fulfillment status
- item info: name, price, image + tracking url
- item info: name, price, image + tracking url
- item info: name, price, image + tracking url

### How to display a tracking url of an item?

```
{% if customer %}
    // customer is logged in
    {% for order in customer.orders %}
        // show order info
        {%- for line_item in order.line_items -%}
            // show item info
            {%- if line_item.fulfillment.tracking_url -%}
                <a href="{{ line_item.fulfillment.tracking_url }}">
                    {{ 'customer.order.track_shipment' | t }}
                </a>
            {% endif %}
        {% endfor %}
    {% endfor %}
{% else %}
    // redirect to login page
{% endif %}
```

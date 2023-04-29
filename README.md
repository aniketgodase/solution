import requests
import json

def FindAlternateGroups(store_domain):
    products_url = f"{store_domain}/collections/all/products.json"
    page = 1
    products = []

    
    while True:
        response = requests.get(f"{products_url}?page={page}")
        page_data = json.loads(response.text)
        page_products = page_data.get('products', [])
        if not page_products:
            break
        products.extend(page_products)
        page += 1

    alternates = []
    visited = set()

    for i in range(len(products)):
        if i not in visited:
            current_product = products[i]
            current_alternates = []
            visited.add(i)

            for j in range(i+1, len(products)):
                if j not in visited:
                    other_product = products[j]
                    if current_product['title'] == other_product['title']:
                        current_alternates.append(other_product['handle'])
                        visited.add(j)

            if len(current_alternates) > 0:
                current_alternates.append(current_product['handle'])
                alternates.append({"product alternates": current_alternates})

    return json.dumps(alternates, indent=4)


store_domain = "https://www.boysnextdoor-apparel.co"
result = FindAlternateGroups(store_domain)
print(result)

output
[
    {
        "product alternates": [
            "boysnextdoor-coach-shirt-jacket-white-1",
            "boysnextdoor-coach-shirt-jacket-white"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-patchwork-sweater-green-1",
            "boysnextdoor-patchwork-sweater-green-2",
            "boysnextdoor-patchwork-sweater-green"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-patchwork-sweater-navy-1",
            "boysnextdoor-patchwork-sweater-navy-2",
            "boysnextdoor-patchwork-sweater-navy"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-round-collar-shirt-grey-1",
            "boysnextdoor-round-collar-shirt-grey"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-round-collar-shirt-white-1",
            "boysnextdoor-round-collar-shirt-white"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-slim-chino-pants-black-new",
            "boysnextdoor-slim-chino-pants-black"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-slim-chino-pants-green-new",
            "boysnextdoor-slim-chino-pants-green"
        ]
    },
    {
        "product alternates": [
            "boysnextdoor-wide-pocket-tee-white-new",
            "boysnextdoor-wide-pocket-tee-white"
        ]
    },
    {
        "product alternates": [
            "fred-perry-crewneck-sweatshirt-black-1",
            "fred-perry-crewneck-sweatshirt-black"
        ]
    },
    {
        "product alternates": [
            "gramicci-beams-nylon-fising-short-pants-1",
            "gramicci-beams-nylon-fising-short-pants"
        ]
    },
    {
        "product alternates": [
            "norse-projects-holger-tab-organic-cotton-t-shirt-1",
            "norse-projects-holger-tab-organic-cotton-t-shirt-2",
            "norse-projects-holger-tab-organic-cotton-t-shirt"
        ]
    },
    {
        "product alternates": [
            "the-north-face-key-keeper-long-1",
            "the-north-face-key-keeper-long-2",
            "the-north-face-key-keeper-long"
        ]
    },
    {
        "product alternates": [
            "the-north-face-protector-daily-short-1",
            "the-north-face-protector-daily-short-2",
            "the-north-face-protector-daily-short-3",
            "the-north-face-protector-daily-short-4",
            "the-north-face-protector-daily-short"
        ]
    },
    {
        "product alternates": [
            "the-north-face-utility-tote-1",
            "the-north-face-utility-tote-2",
            "the-north-face-utility-tote-3",
            "the-north-face-utility-tote"
        ]
    }

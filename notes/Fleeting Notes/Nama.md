# Nama
Created: 2023-05-06 14:23
Tags: 
____

Factory pattern
Aggregation pattern

  
  
* clients (12)
* schema factory ?
* web_url ?
* translator
* widgets

-   Get response
	-   Check version
	-   Validate request
	-   Search
		-   Get filter extraction JLI list
		-   Aggregator.aggregate
			- Get before post list widget
			-   Get post list widgets
			-   After post list widget
		-   monitor



toolbox builder
banner builder
filter chips builder
general builder








```python


class AbstractResponseBuilder:
    def __init__(self, request: Request, 
    data, version, city_id, 
    cat_slug, schema,
    global_config, 
    response_holder: BuilderResponseHolder):
    
    @property
    def phone_number(self):
        return self.client_info.phone_number

    @property
    def platform(self):
        return self.client_info.platform

    @property
    def device_id(self):
        return self.client_info.device_id

    @property
    def os_version(self):
        return self.client_info.os_version

    @property
    def client_version(self):
        api_version = self.client_info.api_version
        return self.mapped_api_version if api_version == 0 else api_version

    @property
    def page(self):
        return self.data.get('page')

    @property
    def category(self):
        return self.jli.get(FILTER_CATEGORY, {}).get(KEY_VALUE, CATEGORY_ROOT)

    @property
    def is_multi_city_search(self):
        return len(self.jli.get("cities", [])) > 1

    @property
    def multi_city_search_cities(self):
        return self.jli.get("cities", [])

    @property
    def cities(self):
        return list(self.jli.get("cities", []) or [self.city_id])

    @property
    def districts(self):
        return self.jli.get("districts", {}).get("vacancies", [])

    async def perform_request(self):
        if not self.is_applicable():
            return

        with MonitorBuilderResponseTime(self._builder_name):
            coroutine = self.get_request_coroutine()
            if iscoroutine(coroutine):
                response = await coroutine
                self.save_response(response)

    def is_applicable(self):
        return True

    def save_response(self, builder_result):
        raise NotImplementedError()

    def get_request_coroutine(self):
        raise NotImplementedError()

    def get_widgets(self):
        return []

    def get_toolbox_widgets(self):
        return []

    def get_web_toolbox_widgets(self):
        return []

    def get_filter_chips(self):
        return []

    @property
    def client_platform(self):
        """DivarWidgets compatible client platform"""
        platform = self.client_info.platform

        if platform == 'I':
            return ClientPlatform.IOS
        elif platform == 'A':
            return ClientPlatform.ANDROID
        elif platform == 'W':
            return ClientPlatform.WEB
        return ClientPlatform.UNKNOWN

    @property
    def _builder_name(self):
        return type(self).__name__

```
_____
##### References
1.


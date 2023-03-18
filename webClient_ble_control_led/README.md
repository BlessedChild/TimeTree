# Introduction

This is a demo for controling the duty of the led/rgb through the web of Chrome(Web bluetooth)

# Key

GATTS SERVICE and BLE CONTROL

key for GATTS SERVICE, methods of ble control will be called when ESP_GATTS_CONNECT_EVT or ESP_GATTS_DISCONNECT_EVT is triggered.
``` c
case ESP_GATTS_CONNECT_EVT: {
    esp_ble_conn_update_params_t conn_params = {0};
    memcpy(conn_params.bda, param->connect.remote_bda, sizeof(esp_bd_addr_t));
    /* For the IOS system, please reference the apple official documents about the ble connection parameters restrictions. */
    conn_params.latency = 0;
    conn_params.max_int = 0x20;    // max_int = 0x20*1.25ms = 40ms
    conn_params.min_int = 0x10;    // min_int = 0x10*1.25ms = 20ms
    conn_params.timeout = 400;    // timeout = 400*10ms = 4000ms
    ESP_LOGI(GATTS_TAG, "ESP_GATTS_CONNECT_EVT, conn_id %d, remote %02x:%02x:%02x:%02x:%02x:%02x:",
                param->connect.conn_id,
                param->connect.remote_bda[0], param->connect.remote_bda[1], param->connect.remote_bda[2],
                param->connect.remote_bda[3], param->connect.remote_bda[4], param->connect.remote_bda[5]);
    gl_profile_tab[PROFILE_A_APP_ID].conn_id = param->connect.conn_id;
    //start sent the update connection parameters to the peer device.
    esp_ble_gap_update_conn_params(&conn_params);
    //open led
    open_led();
    ESP_LOGI(GATTS_TAG, "open led");
    break;
}
case ESP_GATTS_DISCONNECT_EVT:
    ESP_LOGI(GATTS_TAG, "ESP_GATTS_DISCONNECT_EVT, disconnect reason 0x%x", param->disconnect.reason);
    esp_ble_gap_start_advertising(&adv_params);
    //close led
    close_led();
    ESP_LOGI(GATTS_TAG, "close led");
    break;
```

key for BLE CONTROL, timer configuration and channel configuration.
``` c
static void example_ledc_init(void)
{
    // Prepare and then apply the LEDC PWM timer configuration
    ledc_timer_config_t ledc_timer = {
        .speed_mode       = LEDC_MODE,
        .timer_num        = LEDC_TIMER,
        .duty_resolution  = LEDC_DUTY_RES,
        .freq_hz          = LEDC_FREQUENCY,  // Set output frequency at 5 kHz
        .clk_cfg          = LEDC_AUTO_CLK
    };
    ESP_ERROR_CHECK(ledc_timer_config(&ledc_timer));

    // Prepare and then apply the LEDC PWM channel configuration
    ledc_channel_config_t ledc_channel = {
        .speed_mode     = LEDC_MODE,
        .channel        = LEDC_CHANNEL_0,
        .timer_sel      = LEDC_TIMER,
        .intr_type      = LEDC_INTR_DISABLE,
        .gpio_num       = LEDC_OUTPUT_IO_0,
        .duty           = 0, // Set duty to 0%
        .hpoint         = 0
    };
    ESP_ERROR_CHECK(ledc_channel_config(&ledc_channel));

    // Prepare and then apply the LEDC PWM channel configuration
    ledc_channel_config_t ledc_channel_1 = {
        .speed_mode     = LEDC_MODE,
        .channel        = LEDC_CHANNEL_1,
        .timer_sel      = LEDC_TIMER,
        .intr_type      = LEDC_INTR_DISABLE,
        .gpio_num       = LEDC_OUTPUT_IO_1,
        .duty           = 0, // Set duty to 0%
        .hpoint         = 0
    };
    ESP_ERROR_CHECK(ledc_channel_config(&ledc_channel_1));

    // Prepare and then apply the LEDC PWM channel configuration
    ledc_channel_config_t ledc_channel_2 = {
        .speed_mode     = LEDC_MODE,
        .channel        = LEDC_CHANNEL_2,
        .timer_sel      = LEDC_TIMER,
        .intr_type      = LEDC_INTR_DISABLE,
        .gpio_num       = LEDC_OUTPUT_IO_2,
        .duty           = 0, // Set duty to 0%
        .hpoint         = 0
    };
    ESP_ERROR_CHECK(ledc_channel_config(&ledc_channel_2));
}
```

key for BLE CONTROL, change PWM signal.
``` c
static void open_led(void)
{
    // Set the LEDC peripheral configuration
    example_ledc_init();

}

static void close_led(void)
{
    // Set duty to 100%
    ESP_ERROR_CHECK(ledc_set_duty(LEDC_MODE, LEDC_CHANNEL_0, LEDC_DUTY * 2));
    // Set duty to 100%
    ESP_ERROR_CHECK(ledc_set_duty(LEDC_MODE, LEDC_CHANNEL_1, LEDC_DUTY * 2));
     // Set duty to 100%
    ESP_ERROR_CHECK(ledc_set_duty(LEDC_MODE, LEDC_CHANNEL_2, LEDC_DUTY * 2));
    // Update duty to apply the new value
    ESP_ERROR_CHECK(ledc_update_duty(LEDC_MODE, LEDC_CHANNEL_0));
    // Update duty to apply the new value
    ESP_ERROR_CHECK(ledc_update_duty(LEDC_MODE, LEDC_CHANNEL_1));
    // Update duty to apply the new value
    ESP_ERROR_CHECK(ledc_update_duty(LEDC_MODE, LEDC_CHANNEL_2));
}
```

# OUTPUT

* enter about://bluetooth-internals/#devices of Chrome and click the "Start Scan" button
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/webClient_ble_control_led/scan_divice.jpg)

* Find the device named "ESP_GATTS_DEMO" and, it is automatically connected to the device when after click the "Inspect" button，which is the same as clicking the "Connect" button.（After clicking the "Connect"）
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/webClient_ble_control_led/after_click_connect.jpg)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/webClient_ble_control_led/connect_event_close_led.jpg)

* After clicking the "Disconnect"
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/webClient_ble_control_led/after_click_disconnect.jpg)
![avatar](https://github.com/BlessedChild/TimeTree/blob/main/sources/webClient_ble_control_led/disconnect_event_close_led.jpg)

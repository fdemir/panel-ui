<script>
  import Icon from "svelte-awesome";
  import moment from "moment";
  import { onMount, onDestroy } from "svelte";

  import { showNetworkErrorOnCatch, isPageInitialized } from "../Store";
  import ApiUtil from "../pano/js/api.util";
  import tooltip from "../pano/js/tooltip.util";

  import { faBell, faDotCircle } from "@fortawesome/free-regular-svg-icons";
  import ConfirmRemoveAllNotificationsModal from "../components/modals/ConfirmRemoveAllNotificationsModal.svelte";

  let notificationProcessID = 0;
  let notifications = [];

  let checkTime = 0;
  let interval;

  Array.prototype.insert = function (index, item) {
    this.splice(index, 0, item);

    return this;
  };

  Array.prototype.remove = function (index) {
    this.splice(index, 1);

    return this;
  };

  function setNotifications(newNotifications) {
    if (notifications.length === 0 || newNotifications.length === 0)
      notifications = newNotifications;
    else {
      const listOfFilterIsNotificationExists = [];

      newNotifications.forEach((item, index) => {
        listOfFilterIsNotificationExists[index] = notifications.filter(
          (filterItem) => filterItem.id === item.id
        );
      });

      newNotifications.forEach((item, index) => {
        if (listOfFilterIsNotificationExists[index].length === 0) {
          notifications = notifications.insert(index, item);
        }
      });

      notifications.forEach((item, index) => {
        const newArrayOfFilter = newNotifications.filter(
          (filterItem) => filterItem.id === item.id
        );

        if (newArrayOfFilter.length === 0) {
          notifications = notifications.remove(index);
        }
      });
    }
  }

  function getNotifications(id) {
    showNetworkErrorOnCatch((resolve, reject) => {
      ApiUtil.get("panel/notifications")
        .then((response) => {
          if (notificationProcessID === id) {
            if (response.data.result === "ok") {
              setNotifications(response.data.notifications);

              isPageInitialized.set(true);
            }

            setTimeout(() => {
              if (notificationProcessID === id) {
                startNotificationsDown();
              }
            }, 1000);
          }

          resolve();
        })
        .catch(() => {
          reject();
        });
    });
  }

  function startNotificationsDown() {
    notificationProcessID++;

    const id = notificationProcessID;

    getNotifications(id);
  }

  startNotificationsDown();

  onMount(() => {
    interval = setInterval(() => {
      checkTime += 1;
    }, 1000);
  });

  onDestroy(() => {
    notificationProcessID++;

    clearInterval(interval);
  });

  function getTime(check, time, locale) {
    return moment(time).fromNow();
  }
</script>

<div class="container">
  <!-- Action Menu -->
  <div class="row mb-3">
    <div class="col-md-4 col-6">
      <h3 class="text-muted badge badge-lightprimary panel-subtitle mb-0">
        Bildirimler
      </h3>
    </div>
    {#if notifications.length !== 0}
      <div class="col text-right">
        <button
          type="button"
          class="btn btn-link text-danger"
          data-target="#ConfirmRemoveAllNotificationsModal"
          data-toggle="modal">
          <span class="d-md-inline d-none">Tümünü Sil</span>
        </button>
      </div>
    {/if}
  </div>

  <!-- All Notifications -->

  <div class="card">
    <div class="card-body">
      <div
        class="d-flex flex-column justify-content-center align-items-center
        border rounded">
        {#each notifications as notification, index (notification)}
          <div class="d-flex flex-row align-items-center w-100 border-bottom">
            <a
              href="javascript:void(0);"
              class="dropdown-item d-flex flex-row py-2"
              class:notification-unread="{notification.status === 'NOT_READ'}">
              <div class="col-auto pl-0">
                <Icon data="{faDotCircle}" class="text-primary" />
              </div>
              <div class="col">
                <span class="text-wrap text-dark">{notification.type_ID}</span>
                <small class="text-gray d-block">
                  {getTime(checkTime, parseInt(notification.date), "")}
                </small>
              </div>
            </a>
            <button
              class="btn btn-link text-danger mx-2"
              use:tooltip="{['right', 'Bildirimi Sil']}">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
        {/each}

        {#if notifications.length === 0}
          <div
            class="d-flex flex-column align-items-center justify-content-center">
            <Icon data="{faBell}" scale="3" class="text-glass m-3" />
            <p class="text-gray">Bildirim yok.</p>
          </div>
        {/if}
      </div>

      {#if notifications.length !== 0}
        <nav class="pt-3">
          <ul class="pagination pagination-sm mb-0 justify-content-start">
            <!--          :class="{ 'disabled': page === 1 }"-->
            <!--          @click="routePage(1)"-->
            <li class="page-item">
              <a
                class="page-link"
                href="javascript:void(0);"
                title="Önceki Sayfa">
                <span aria-hidden="true">&laquo;</span>
              </a>
            </li>

            <!--          :class="{ 'active': index === page }"-->
            <!--          :key="index"-->
            <!--          @click="routePage(index)"-->
            <!--          v-for="index in total_page"-->
            <li class="page-item">
              <a
                class="page-link"
                href="javascript:void(0);"
                v-if="page !== index">
                <!--                {{index}}-->
              </a>
              <!--            {{index}}-->
              <a class="page-link" href="javascript:void(0);">?</a>
              <!-- v-if="page === index" -->
            </li>

            <!--          :class="{ 'disabled': page === total_page }"-->
            <!--          @click="routePage(total_page)"-->
            <li class="page-item">
              <a
                class="page-link"
                href="javascript:void(0);"
                title="Sonraki Sayfa">
                <span aria-hidden="true">&raquo;</span>
              </a>
            </li>
          </ul>
        </nav>
      {/if}
    </div>
  </div>
</div>

<ConfirmRemoveAllNotificationsModal />

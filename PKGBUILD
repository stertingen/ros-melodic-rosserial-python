pkgdesc="ROS - A Python-based implementation of the rosserial protocol."
url='https://wiki.ros.org/rosserial_python'

pkgname='ros-melodic-rosserial-python'
pkgver='0.8.0'
arch=('any')
pkgrel=2
license=('BSD')

ros_makedepends=(
	ros-melodic-catkin
)

makedepends=(
	'cmake'
	'ros-build-tools'
	${ros_makedepends[@]}
)

ros_depends=(
	ros-melodic-rospy
    ros-melodic-rosserial-msgs
    ros-melodic-diagnostic-msgs
)

depends=(
    'python-pyserial'
	${ros_depends[@]}
)

_dir="rosserial-${pkgver}/rosserial_python"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-drivers/rosserial/archive/${pkgver}.tar.gz"
        "fix-reconnection.patch"::"https://patch-diff.githubusercontent.com/raw/ros-drivers/rosserial/pull/445.patch"
        "fix-python3.patch"::"https://patch-diff.githubusercontent.com/raw/ros-drivers/rosserial/pull/469.patch"
        "fix-py3-except.patch"::"https://patch-diff.githubusercontent.com/raw/ros-drivers/rosserial/pull/470.patch")
sha256sums=('e96cdeb81e1c03fb1c5ad85a740cb0a1a0836c52a24c6a5d97c975084b49d576'
            '810c044f7ef6aae65ed624d92fbd27313c2a8ef21c6739202721b5893129622c'
            'e91769fba0c6037362ae0d52717b76ecf26b593c27ee06ad005499b64288738f'
            'd99d7e052b78cb012f7f66c951eb2eac2e170df4a88bc2de805719cfe92f3062')

prepare() {
    cd ${srcdir}/${_dir}/..
    patch -p1 < ${srcdir}/fix-reconnection.patch
    patch -p1 < ${srcdir}/fix-python3.patch
    patch -p1 < ${srcdir}/fix-py3-except.patch
}

build() {
	# Use ROS environment variables.
	source /usr/share/ros-build-tools/clear-ros-env.sh
	[ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

	# Create the build directory.
	[ -d ${srcdir}/build ] || mkdir ${srcdir}/build
	cd ${srcdir}/build

	# Fix Python2/Python3 conflicts.
	/usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

	# Build the project.
	cmake ${srcdir}/${_dir} \
		-DCMAKE_BUILD_TYPE=Release \
		-DCATKIN_BUILD_BINARY_PACKAGE=ON \
		-DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
		-DPYTHON_EXECUTABLE=/usr/bin/python3 \
		-DSETUPTOOLS_DEB_LAYOUT=OFF
	make
}

package() {
	cd "${srcdir}/build"
	make DESTDIR="${pkgdir}/" install
}
